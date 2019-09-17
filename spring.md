ioc是一个容器，帮我们管理所有组件（容器其实就是一个map）

1、依赖注入，如@Autowired：自动赋值

springbean生命周期

Bean的定义——Bean的初始化——Bean的使用——Bean的销毁 

springmvc流程：dispatcherservlet获取http请求，调用Handler mapping获取响应的控制器来处理请求，处理完成之后再又视图解析器获取视图，最后控制器再将模型唱颂歌视图，然后会先到浏览器上。





项目：ATD（Architecture te'chnical debt） Manager

提出了六个架构视点来系统地记录ATD，即ATD细节视点（ATD Detail Viewpoint）、ATD决策视点(ATD Decision Viewpoint)、ATD相关组件视点(ATD-related Component Viewpoint）、ATD分布视点(ATD Distribution Viewpoint）、ATD利益相关者参与视点(ATD Stakeholder Involvement Viewpoint）以及ATD演化视点(ATD Chorological Viewpoint）

研究交替修改指标与软件的缺陷密度之间的关系

交替修改指标：它包括了一系列可能影响源文件被交替修改程度的因素，比如，交替修改数占源文件修改总数的比例、产生交替修改的提交者数目占参与源文件修改的提交者总数的比例、以及每个交替修改被干扰修改影响的程度

缺陷密度：缺陷数/代码行数

问题跟踪系统导出开源软件的issues

通过git或svn获取开源项目的提交记录

斯皮尔曼相关系数

了解这种交替形式的修改是否会导致开源软件出现更严重的错误倾向以及技术债务累积的问题，这将有助于开源软件的参与者在项目演化过程中随时掌握当前软件的质量水平。若修改后的质量出现恶化，也可及时采取进一步的措施管理并控制项目中技术债务的累积，提高开源软件的质量，从而最大程度地发挥群体自由开发模式的优势作用。