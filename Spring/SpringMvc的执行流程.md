## SpringMvc的执行流程

客户端发出请求。

1. DispatcherServlet拦截，处理请求到HandlerMapping。
2. handlerMapping映射请求到对应的方法，并返回处理器执行链给DispatcherServlet。（如果由拦截器则共同返回）
3. DispatcherServlet根据handlerMapping 的返回结果（处理器执行链），找到HanderAptor执行处理器执行链的方法(处理请求参数），然后找到对应的handler然后执行方法，然后再将结果响应给HandlerAdaptor（处理返回值）。
4. HandlerAdaptor处理后将ModeAndView返回给DispatcherServlet.
5. DispatcherServlet再将MoveAndView，返回给ViewResolver视图解析器，视图解析器返回给DispatcherServlet,真正的View对象，经过渲染展示视图。
6. DispatcjerServlet返回结果给请求。

![image-20241015180159298](images/SpringMvc的执行流程.assets/image-20241015180159298.png)



但是现在大部分都是前后端分离开发，流程由些变化。

在HanderAptor处理handler时，加上@RequestBody。通过HttpMessage来处理返回结果,转化为json对象.返回给前端。

![image-20241015180250990](images/SpringMvc的执行流程.assets/image-20241015180250990.png)



