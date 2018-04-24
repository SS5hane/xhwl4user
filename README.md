# 招娉网站对用户前端项目日志
## 运行步骤：
- npm install
- 将根目录下的element-ui.commom.js放在node_modules\element-ui\lib下覆盖原文件
- npm run dev

**请前端人员在更改项目时以如下格式写明自己正在做以及做完的组件或者其他文件**
```markdown
### 18年03月31日08时-18年03月31日09时 
  
  更新内容：创建项目 
  
  更新人员：李新阳
```
## 4月日志
 
 ### 18年04月02日12时-18年04月02日16时 
  
  更新内容：重编部分html为组件，在main.js里添加路由，并搭建基本的路由逻辑path及对应component。
  - 其中更新的组件有：
    + 用户界面的页面头部（XhwlHeader）
    + 用户界面首页的中部（Home）
    + **用户界面的页面底部遇到一些问题暂未完成**
  - 添加的路由（暂时指向空组件）有：
    + 首页（）
    + 社会招聘 （/Social）
    + 校园招聘分
        + 招聘职位（/Campus/Post）
        + 招聘流程（/Campus/Procedure）
        + 培养计划（/Campus/Plan）
    + 实习生招聘（/Trainee）
    + 关于兴海物联（/AboutUs）
  
  更新人员：李新阳
  
  ### 18年04月03日12时-18年04月02日18时 
  
  更新内容：更新部分组件以及未创建简历时提示创建简历的页面，添加部分路由。
  - 其中更新的组件有：
    + 搜索栏（Selector）
    + 用户界面头部的登录后下拉框及提示信息（更新了XhwlHeader）
    + 未创建简历时提示创建简历的提示动画组件（MyResume）
    +  **暂时使用的职位卡片（等待更新）**
  - 添加的路由有：
    + 我的简历（/MyResume）
    
  更新人员：李新阳
  
   ### 18年04月05日12时-18年04月06日12时 
  
  更新内容：更新部分组件以及部分页面毛胚，添加部分路由。
  - 其中更新的组件有：
    + 填写表单页面（ResumeForm）
    + 表单页面子组件有：
       + 基本信息
       + 教育经历
       + **其余八个表单已搭建完成但未修改其中的表单内容**
    + 投递信息（MyJobApplication）
  - 添加的路由有：
    + 我的投递（/MyJobApplication）
    + 填写简历及其子路由（/ResumeForm/1~/ResumeForm/10）
    
  更新人员：李新阳
  
   ### 18年04月07日12时-18年04月08日21时 
	
   更新内容：更新部分组件。
  - 其中更新的组件有：
    + 所有表单已经更新为原型所写
    + 更新未创建简历时的动画
  
    
  更新人员：李新阳
  
   ### 18年04月09日10时-18年04月09日16时 
	
   更新内容：更新部分组件。
  - 其中更新的组件有：
    + 职位卡片已经更改为可以使用的版本
    + 更新查看职位时的动画
  
    
  更新人员：李新阳
  
   ### 18年04月10日10时-18年04月10日20时 
	
   更新内容：更新首页页面为设计助理所设计样图。
  
  更新人员：李新阳
  
   ### 18年04月11日10时-18年04月10日20时 
	
   更新内容：更新填写简历页面为设计助理所设计样图（部分）。
  
  更新人员：李新阳
  
  ### 18年04月10日-18年04月12日8时 
	
  更新内容：更新绑定简历部分的9个表单的v-model。
  
  更新人员：王圣哲
  
   ### 18年04月11日10时-18年04月10日20时 
	
   更新内容：更新填写简历页面为设计助理所设计（基本完成）并纠正合并后的部分错误。
  
  更新人员：李新阳
  
   ### 18年04月13日10时-18年04月14日11时 
	
   更新内容：更新首页页面为设计助理所设计样图（完整、包括css），进行了第一次axios的登录以及加入头部token来get简历信息。
  
  更新人员：李新阳
  
  ### 18年04月16日10时-18年04月16日14时 
   
  更新内容：进行axios示例的编写（分别对应两类：普通信息类和经历类），更改了经历页的前端响应。
   - 加入基础信息表的除照片上传外的axios
   - 加入教育经历表的所有增加，编辑，删除逻辑以及axios

  更新人员：李新阳
  

   ### 18年04月14日8时-18年04月15日21时 
	
   更新内容：
   - 新增加FormBasicInformation的表单验证
   - 修改一部分表单中不合理的内容
   - 新增验证表单用的util文件夹和validate.js文件

  更新人员：王圣哲
  
   ### 18年04月18日10时-18年04月20日9时 
   
  更新内容：重构了项目结构，进行一定的结构梳理和分类，更改配色，更新了get逻辑使得填写简历页面可以同步刷新。


  更新人员：李新阳
  
   ### 18年04月16日8时-18年04月21日21时 
	
  更新内容：更新所有表单的axios和表单验证
  
  
  更新人员：王圣哲
  
 ### 18年04月22日10时-18年04月22日12时 
   
  更新内容：加入上传照片的功能

  更新人员：李新阳
  
   ### 18年04月23日14时-18年04月23日20时 
   
  更新内容：增加表单的nextTick()，增加日期验证，修改自我描述的Bug

  更新人员：王圣哲
  
   ### 18年04月24日8时-18年04月24日10时 
   
  更新内容：添加结束日期大于起始日期的表单验证
           修改表单解决添加新字条时复制上一条的bug

  更新人员：王圣哲
  

