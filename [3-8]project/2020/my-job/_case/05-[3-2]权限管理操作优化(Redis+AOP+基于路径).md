# [3-2]权限管理优化(Redis+AOP+基于路径)

> @todo

- 基于路径的动态权限控制

# 扫描

- 工具类

```java
@Slf4j
public class PermissionUrlScanUtils {
    public static Multimap<String, UrlModulesParam> modules = ArrayListMultimap.create();
    public static List<UrlModulesParam> moduleKeys = new ArrayList<>();

    public static String YLMM_COMMON_PARENT_PATH = "/ylmm/api"; // 自定义

    public static List<SysMenu> scan(String packageName) {
        ClassPathScanningCandidateComponentProvider provider = new ClassPathScanningCandidateComponentProvider(true);
        provider.addIncludeFilter(new AnnotationTypeFilter(Api.class));
//        provider.addIncludeFilter(new AssignableTypeFilter(PreAuthorize.class));
        Set<BeanDefinition> beanDefinitionSet = provider.findCandidateComponents(packageName);

        for (BeanDefinition definition : beanDefinitionSet) {
            scanClass(definition.getBeanClassName());
        }

        return analyze();
    }

    public static void scanClass(String classFullPath) {
        try {
//            Class clazz = ClassLoader.getSystemClassLoader().loadClass(classFullPath);

            Class clazz = Thread.currentThread().getContextClassLoader().loadClass(classFullPath);

            Annotation a = clazz.getAnnotation(Api.class);
            Annotation b = clazz.getAnnotation(RequestMapping.class);
            if (a == null) {
                return;
            }

            UrlModulesParam paramClazz = new UrlModulesParam(
                    ((Api) a).tags()[0],
                    ((RequestMapping) b).value()[0]);

            moduleKeys.add(paramClazz);

            log.info("class:" + paramClazz);
            Method[] methods = clazz.getDeclaredMethods();

            UrlModulesParam param = null;
            for (Method method : methods) {
                Annotation[] annotations = method.getDeclaredAnnotations();

                param = new UrlModulesParam();

                for (Annotation annotation : annotations) {
                    if (annotation instanceof PreAuthorize) {
                        param.setPath(((PreAuthorize) annotation).value()); // 要根据自定义注解的字段情况
                    }

                    if (annotation instanceof ApiOperation) {
                        param.setName(((ApiOperation) annotation).value());
                    }
                }

                if (StrUtil.isNotEmpty(param.getName()) && StrUtil.isNotEmpty(param.getPath())) {
                    paramClazz.getChildren().add(param);
                    modules.put(paramClazz.getName(), param);
                    log.info("method:" + param);
                }

            }

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
            log.info("ClassNotFoundException:" + classFullPath);
        }
    }

    public static List<SysMenu> analyze() {
        List<SysMenu> sysModules = new ArrayList<>();
        SysMenu sysModuleParent = null;
        SysMenu sysModule = null;

        Set<String> urlSet = new HashSet<>();

        //排序模块显示顺序
        // Collections.sort(moduleKeys);

        List<UrlModulesParam> subModules = null;
        for (UrlModulesParam module : moduleKeys) {
            String group = module.getName();

            if (urlSet.contains(module.getPath())) {
                log.info("[group]" + group + " [name]" + module.getName() + " [path]" + module.getPath() + "已经存在");
            } else {
                sysModuleParent = new SysMenu();

                sysModuleParent.setMenuName(module.getName());
                sysModuleParent.setPath(module.getPath());
      
                sysModuleParent.setParentPath(YLMM_COMMON_PARENT_PATH);
                sysModuleParent.setMenuType("1");
                sysModules.add(sysModuleParent);
                urlSet.add(module.getPath());
            }

            subModules = (List<UrlModulesParam>) modules.get(module.getName());
            // Collections.sort(subModules);

            for (UrlModulesParam sub : subModules) {
                if (urlSet.contains(sub.getPath())) {
                    log.info("[group]" + group + " [name]" + sub.getName() + " [path]" + ((sysModuleParent.getPath().replace("*", "") + sub.getPath()).replace("//", "/")) + "已经存在");
                } else {
                    sysModule = new SysMenu();
                    sysModule.setMenuName(module.getName() + "-" + sub.getName());
                    sysModule.setPath((sub.getPath()).replace("//", "/"));
                    sysModule.setMenuType("2");
                    sysModule.setParentPath(module.getPath());

                    sysModules.add(sysModule);
                    urlSet.add(sysModule.getPath());
                }

            }
        }

        return sysModules;
    }

}
```

- 测试

```java
List<SysMenu> list = PermissionUrlScanUtils.scan("com.pm.szptb.nbszh.web.admin.controller");
```

# 参考

- 使用Redis+AOP优化权限管理功能，这波操作贼爽！
  - http://www.macrozheng.com/#/technology/redis_permission

```
把用户信息和用户资源信息存入到Redis中去

缓存操作不该影响正常业务逻辑，使用AOP来统一处理缓存操作中的异常

@Pointcut("execution(public * com.macro.mall.portal.service.*CacheService.*(..)) || execution(public * com.macro.mall.service.*CacheService.*(..))")
```

- 手把手教你搞定权限管理，结合Spring Security实现接口的动态权限控制！
  - https://mp.weixin.qq.com/s/nvKKNSJuIrGuHeJkUeO7rw

- 基于 token 的多平台身份认证架构设计
  - https://juejin.cn/post/6942684540431237133

- 史上最强的权限系统设计攻略(上)、基础概念、RBAC以及ABAC模型
  - https://juejin.cn/post/6941734947551969288