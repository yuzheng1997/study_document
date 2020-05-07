# MyBatis 插件

Mybatis允许在已映射语句执行过程中的某一点进行拦截调用。包括如下几个接口和方法

- Executor
  - update
  - query
  - flushStatements
  - commit
  - rollback
  - getTransaction
  - close
  - isClosed
- ParameterHandler
  - getParameterObject
  - setParameters
- ResultSetHandler
  - handleResultSets
  - handleCursorResultSets
  - handleOutputParameters
- StatementHandler
  - prepare
  - parameterize
  - batch
  - update
  - query

## 拦截器接口

```java
package dy.study.mybatisstudy.common.Interceptor;

import org.apache.ibatis.plugin.Interceptor;
import org.apache.ibatis.plugin.Invocation;
import org.apache.ibatis.plugin.Plugin;

import java.util.Properties;

/**
 * @Classname MyPageInterceptor
 * @Description TODO
 * @Date 2020/4/25 19:24
 * @Created by Mr.Y
 */
public class MyPageInterceptor implements Interceptor {
    /**
     * 执行时的拦截方法
     * getTarget() 可以获取当前被拦截的对象
     * getMethod() 可以获取当前被拦截的方法
     * proceed() 执行真正的方法
     * @param invocation
     * @return
     * @throws Throwable
     */
    @Override
    public Object intercept(Invocation invocation) throws Throwable {
        return null;
    }

    /**
     *
     * @param target
     * @return
     */
    @Override
    public Object plugin(Object target) {
        /**
         * target 代表要拦截的对象
         * Plugin.wrap会自动判断拦截器的签名和被拦截
         * 的对象是否匹配，只有匹配的才会代理拦截对象
         */
        return Plugin.wrap(target, this);
    }

    /**
     * 用来传递插件的参数，可以通过参数来改变插件的行为
     * 参数可以通过xml的方式
     * spring配置类的方式
     * @param properties
     */
    @Override
    public void setProperties(Properties properties) {

    }
}

```

当配置多个拦截器时，MyBatis会遍历所有拦截器，按顺序执行拦截器的plugin方法，被拦截的对象会被层层代理。在执行拦截对象的方法时，会一层层的调用拦截器，拦截器通过invocation.process()调用下一层的方法，直到真正的方法被执行。