## 导入示例
1. 打开eclipse
2. `File` -> `import` -> `general` -> `Existing Project into Workspace` -> `Browse` -> `$POLARION_HOME/polarion/SDK/examples/servlet`


## 开发自己的plug-in
1. `File` -> `New` -> `Project` -> `Plug-in Project` 
2. 设置项目名为：com.polarion.example.servlet(取消`Generate an activator`)
3. 打开`MANIFEST.MF`,添加依赖`com.polarion.portal.tomcat`和`com.polarion.alm.tracker`
4. runtime中新建，输入servlet.jar
5. 在Extensions中添加com.polarion.portal.tomcat.webapps
6. 在之前的webapp中
