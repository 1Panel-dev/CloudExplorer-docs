# 开发文档
## 1 环境准备

<<<<<<< HEAD
!!! Abstract ""
    本节介绍如何搭建 CloudExplorer Lite 开发环境。

## 1 前端环境准备
=======
### 1.1 前端环境
>>>>>>> 451152a3d219b98610d9c4221e1b764fee4dbf0b

!!! Abstract ""
    - 安装 [node](https://nodejs.org/)
    - 启用 corepack
  
        - 当 Node.js >=16.10
        ```bash
        corepack enable
        ```

        - 当 Node.js <16.10  
        ```bash
        npm i -g corepack
        ```


### 1.2 后端环境
!!! Abstract ""
    - 安装 [JDK17](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)
    - 安装 [Maven](https://maven.apache.org/download.cgi)
    - 安装 [Docker](https://www.docker.com/) (可选)


## 2 开发准备
### 2.1 VMware相关依赖
!!! Abstract ""
    请在项目根目录执行
    ``` bash
    ./mvnw initialize
    ```
### 2.2 本地配置
!!! Abstract ""
    若要项目启动，需要准备配置文件及目录

    - 初始化目录及文件
    ```properties
    mkdir -p /opt/cloudexplorer/conf
    mkdir -p /opt/cloudexplorer/apps/extra/
    mkdir -p /opt/cloudexplorer/logs

    touch /opt/cloudexplorer/apps/extra/modules
    touch /opt/cloudexplorer/cloudexplorer.properties
    ```

    - 配置`cloudexplorer.properties`，根据实际情况配置文件中的数据库访问地址等参数
    ```bash
    ce.datasource.url=jdbc:mysql://localhost:3306/ce?autoReconnect=false&useUnicode=true&characterEncoding=UTF-8&characterSetResults=UTF-8&zeroDateTimeBehavior=convertToNull&serverTimezone=Asia/Shanghai
    ce.datasource.password=password
    ce.datasource.username=root
    
    #quartz定时任务的数据库可不配置，默认使用ce的数据库配置
    ce.datasource.quartz.url=jdbc:mysql://localhost:3306/quartz?autoReconnect=false&useUnicode=true&characterEncoding=UTF-8&characterSetResults=UTF-8&zeroDateTimeBehavior=convertToNull&serverTimezone=Asia/Shanghai
    ce.datasource.quartz.password=password
    ce.datasource.quartz.username=root
    
    eureka.client.service-url.defaultZone=http://127.0.0.1:8761/eureka/
    eureka.instance.prefer-ip-address=true
    eureka.instance.hostname=eureka
    
    server.max-http-header-size=4086KB
    
    spring.redis.host=localhost
    spring.redis.port=6379
    spring.redis.database=1
    spring.redis.password=redis_password
    
    # ELASTICSEARCH
    spring.elasticsearch.uris=localhost:9200
    #spring.elasticsearch.password=ES_PASSWORD
    #spring.elasticsearch.username=ES_USER
    spring.elasticsearch.connection-timeout=10s
    spring.elasticsearch.read-timeout=30s
    
    #JWT超时时间配置
    jwt.expire.minutes=100
    
    spring.servlet.multipart.max-file-size=50MB
    spring.servlet.multipart.max-request-size=50MB

    logger.level=INFO

    ```

## 3 开发调试
### 3.1 启动前端项目
!!! Abstract ""
    先在根目录执行安装前端需要的依赖
    ```bash
    yarn install
    ```
    根据根目录下`package.json`文件中`script`的命令启动对应模块
    ```bash 
    #以下为快速启动命令

    #启动基座
    yarn dev:base
    #启动management-center
    yarn dev:manage
    #启动vm-service
    yarn dev:vm
    #启动finance-management
    yarn dev:bill
    #启动security-compliance
    yarn dev:security
    #启动operation-analysis
    yarn dev:operation
    ```
### 3.2 启动后端端项目
!!! Abstract ""
    启动对应模块的Application.java启动类即可

## 4 代码相关
### 4.1 接口基本调用参数
!!! Abstract ""
    调用登录以及任意需要认证的接口，在response的header内均会返回当前用户的 JWT token：`CE-TOKEN`

    后端调用接口需要以下header：
    ```
    CE-TOKEN:  登录获取到的 JWT token
    CE-ROLE:   角色(ADMIN,ORGADMIN,USER)
    CE-SOURCE: 组织/工作空间id（ADMIN角色不传）
    ```
    若`CE-ROLE`和`CE-SOURCE`与当前用户不匹配，后端认为就是`ANONYMOUS`角色。

### 4.2 新模块搭建
!!! Abstract ""
    - 打开目录`demo/src/test/java/`下文件 `CreateModuleUtil.java`

    - 修改文件中以下字段为需要生成的新模块信息
    ```java
    /**
     * 模块名
     */
    private static final String NEW_MODULE_NAME = "sample-module";
    
    /**
     * 模块显示名称
     */
    private static final String NEW_MODULE_DISPLAY_NAME = "样例模块";

    /**
     * 模块显示名称（繁体）
     */
    private static final String NEW_MODULE_DISPLAY_NAME_TW = "樣例模塊";

    /**
     * 模块显示名称（英文）
     */
    private static final String NEW_MODULE_DISPLAY_NAME_EN = "Sample Module";

    /**
     * 后端服务端口
     */
    private static final long NEW_MODULE_PORT = 9021;

    /**
     * 开发时前端服务端口
     */
    private static final long NEW_MODULE_FRONTEND_PORT = 5021;

    /**
     * 服务管理端口
     */
    private static final long NEW_MODULE_MANAGEMENT_PORT = 9921;
    
    ```
    - 执行 createModule() 方法，等待执行完成，即会在services目录下根据模版创建出新的模块。

    - 快速生成模块后，可根据需要再修改模块内的 `/模块名.yml` `/backend/src/main/resources/application.yml` 来修改该模块的icon以及菜单序号。

    - 可按需修改模块内的`/backend/src/main/java/com/fit2cloud/ModuleApplication.java`文件名为自己想要的名字。

    - 开始开发你的模块

    - 要启动前端项目请先在项目根目录执行
      ``` bash
      yarn install
      ```
    - 启动前端项目
      ``` bash
      yarn workspace 你的新模块名 run dev
      ```
    - 为了以后启动方便，你也可以在根目录的`package.json`中的`scripts`下添加你的模块
      ``` json
      "scripts": {
        ...
        "dev:你的新模块名": "yarn workspace 你的新模块名 run dev",
        "lint:你的新模块名": "yarn workspace 你的新模块名 run lint",
      },

      ```


### 4.3 后端权限

##### 4.3.1 模块基础权限
!!! Abstract ""
    模块内基础权限配置，在每个模块内的`PermissionConstants.java`中配置`MODULE_PERMISSION_BUILDER`

    ```java
    private static final ModulePermission.Builder MODULE_PERMISSION_BUILDER = new ModulePermission.Builder()
                .group(
                        //用户管理
                        new PermissionGroup.Builder()
                                .id(GROUP.USER)    //权限组
                                .name("i18n_permission_user")
                                .permission(
                                        //查看用户
                                        new Permission.Builder()
                                                .operate(OPERATE.READ)    //权限操作
                                                .name("i18n_permission_user_read")
                                                .role(RoleConstants.ROLE.ADMIN)    //生效的角色
                                                .role(RoleConstants.ROLE.ORGADMIN)
                                )
                                .permission(
                                        //创建用户
                                        new Permission.Builder()
                                                .require(OPERATE.READ)      //该权限的基础权限
                                                .operate(OPERATE.CREATE)    //权限操作
                                                .name("i18n_permission_user_create")
                                                .role(RoleConstants.ROLE.ADMIN)    //生效的角色
                                                .role(RoleConstants.ROLE.ORGADMIN)
                                )
                                
                        //...
                )
                .group(
                        //权限管理
                        new PermissionGroup.Builder()
                                .id(GROUP.ROLE)    //权限组
                                .name("i18n_permission_role")
                                .permission(
                                        new Permission.Builder()
                                                .operate(OPERATE.READ)
                                                .name("i18n_permission_role_read")
                                                .role(RoleConstants.ROLE.ADMIN)
                                                .role(RoleConstants.ROLE.ORGADMIN)
                                )
                )
                //...
                ;
    
    
    ```

##### 4.3.2 后端权限限制
!!! Abstract ""
    支持以下几种方式：
    ```java
    /**
     * 当前模块内的权限判断，第一个参数是权限组group，第二个参数是权限操作operate
     */
    @PreAuthorize("hasCePermission('USER', 'READ')")
    public void mMethod() {
            //...
    }
    
    /**
     * 推荐！
     * 当前模块内的权限判断，参数为权限组与操作的组合: "group:operate"
     */
    @PreAuthorize("hasCePermission('USER:READ')")
    public void mMethod() {
            //...
    }
    
    /**
     * 推荐！
     * 在上面的基础上支持匹配多个权限
     */
    @PreAuthorize("hasAnyCePermission('USER:READ')")
    public void mMethod() {
            //...
    }
    
    /**
     * security默认的方法
     * 需要带上模块名称 "[module_name]group:require+operate" 或 "[module_name]group:operate"
     */
    @PreAuthorize("hasAuthority('[management-center]USER:READ+CREATE')")
    public void mMethod() {
            //...
    }
    
    /**
     * security默认的方法
     * 在上面的基础上支持匹配多个权限
     */
    @PreAuthorize("hasAnyAuthority('[management-center]USER:READ+CREATE')")
    public void mMethod() {
            //...
    }
    
    
    ```

    实现源码见`CeSecurityExpressionRoot.java`

### 4.4 前端权限

##### 4.4.1 html代码中
!!! Abstract ""
    ```html
    <!--
        这里必须使用带模块名的权限格式
        "[module_name]group:require+operate" 或 "[module_name]group:operate"
    -->
    
    <div v-hasPermission="'[management-center]USER:READ+CREATE'"></div>
    
    
    <!-- 也支持数组判断，只要有一个符合就显示 -->
    
    <div v-hasPermission="['[management-center]USER:READ+CREATE','[management-center]USER:READ+EDIT']"></div>
    
    ```

##### 4.4.2 ts代码中
!!! Abstract ""
    ```ts
    import { usePermissionStore } from "@commons/stores/modules/permission";
    const permissionStore = usePermissionStore();
    
    /**
     * 以下方法的返回值即为是否有权限 
     * 这里必须使用带模块名的权限格式
     * "[module_name]group:require+operate" 或 "[module_name]group:operate"
     */
    permissionStore.hasPermission("[vm-service]CLOUD_SERVER:STOP")
    
    /**
     * 也支持数组判断，只要有一个符合就返回true
     */
    permissionStore.hasPermission(["[vm-service]CLOUD_SERVER:START", "[vm-service]CLOUD_SERVER:STOP"])
    
    ```

### 4.5 菜单路由
!!! Abstract ""
    模块内菜单路由配置，在每个模块内的`MenuConstants.java`中配置`MENUS_BUILDER`

##### 4.5.1 单级菜单
!!! Abstract ""
    ```java
        private static final Menus.Builder MENUS_BUILDER = new Menus.Builder()
                .menu(new Menu.Builder()
                        .name("cloud_account")                                       //菜单唯一ID
                        .title("云账号")                                              //菜单显示名称
                        .path("/cloud_account")                                      //菜单路由
                        .componentPath("/src/views/CloudAccount/index.vue")          //路由对应的vue页面，与该项目中前端的frontend目录下的文件路径对应
                        .icon("icon_cloud_outlined")                                 //菜单中展示的icon
                        .order(1)                                                    //菜单中的排序
                        .redirect("/cloud_account/list")                             //如果路径是一个基础路径，其vue组件内有router-view，下面可能包含其他的操作路径，则需要指定默认跳转的下一级子路径
                        .requiredPermission(new MenuPermission.Builder()             //指定访问该菜单所需要的权限
                                .role(RoleConstants.ROLE.ADMIN)
                                .permission(GROUP.CLOUD_ACCOUNT, OPERATE.READ))
                        .childOperationRoute(new Menu.Builder()                       //子路由，配置这个路由不会在前端菜单中展示，结合上面有router-view的组件，可以通过url路径访问到
                                .name("cloud_account_list")
                                .path("/list")                                        //实际会根据父路径拼接成/cloud_account/list进行访问
                                .title("列表")
                                .saveRecent(true)                                     //是否作为最近访问进行记录，在首页最近访问中可以看到
                                .componentPath("/src/views/CloudAccount/list.vue")
                                .requiredPermission(new MenuPermission.Builder()
                                        .role(RoleConstants.ROLE.ADMIN)
                                        .permission(GROUP.CLOUD_ACCOUNT, OPERATE.READ)
                                )
                        )
                        .childOperationRoute(new Menu.Builder()
                                .name("cloud_account_create")
                                .path("/create")
                                .title("创建")
                                .quickAccess(true)                                     //是否支持首页快速访问
                                .saveRecent(true)
                                .quickAccessName("添加云账号")                           //在首页快速访问中展示的名称
                                .quickAccessIcon("icon_cloud_outlined")                //在首页快速访问中展示的图标（已废弃）
                                .componentPath("/src/views/CloudAccount/create.vue")
                                .requiredPermission(new MenuPermission.Builder()
                                        .role(RoleConstants.ROLE.ADMIN)
                                        .permission(GROUP.CLOUD_ACCOUNT, OPERATE.CREATE)
                                )
                        )
                        //...
                )
    ```

##### 4.5.2 有层级的菜单
!!! Abstract ""
    ```java
        private static final Menus.Builder MENUS_BUILDER = new Menus.Builder()
            .menu(new Menu.Builder()
                  .name("system_setting")
                  .title("系统设置")
                  .path("/system_setting")
                  .icon("icon-setting")
                  .order(5)
                  .requiredPermission(new MenuPermission.Builder()       //若有多个角色，需要多次追加requiredPermission
                      .role(RoleConstants.ROLE.ADMIN)
                      .permission(GROUP.PARAMS_SETTING, OPERATE.READ)
                      .permission(GROUP.ABOUT, OPERATE.READ)             //同一角色的MenuPermission.Builder()下可以追加多个permission
                  )
                  .childMenu(new Menu.Builder()                          //次级菜单
                      .name("about")
                      .title("关于")
                      .path("/about")
                      .componentPath("/src/views/About/index.vue")
                      .saveRecent(true)
                      .order(2)
                      .requiredPermission(new MenuPermission.Builder()
                          .role(RoleConstants.ROLE.ADMIN)
                          .permission(GROUP.ABOUT, OPERATE.READ)
                      )
        
                //...
                )
            )
    ```

### 4.6 定时任务
!!! Abstract ""
    文档准备中...

