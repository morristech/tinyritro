## tinyritro
[![Build Status](https://travis-ci.org/hotstu/tinyritro.svg?branch=master)](https://travis-ci.org/hotstu/tinyritro)
轻量的api请求库，基于okhttp3、rxjava2, 使用方式类似retrofit，使用apt，不使用反射,自动完成api请求、json解析，并返回Flowable用于后续调用

## 使用
加入仓库
```java build.gradle
    maven {
        url "https://dl.bintray.com/hglf/maven"
    }
```
加入依赖
```java app/build.gradle
    annotationProcessor 'gitbub.hotstu.tinyritro:tinyritro-compiler:0.0.2'
    implementation 'gitbub.hotstu.tinyritro:tinyritro-lib:0.0.2'
    implementation 'io.reactivex.rxjava2:rxjava:2.1.8'
    implementation 'io.reactivex.rxjava2:rxandroid:2.0.1'
    implementation 'com.squareup.okhttp3:okhttp:3.9.1'
    implementation 'com.google.code.gson:gson:2.8.2'
```
定义接口
```java
@EntryPoint(value = "http://httpbin.org")
public interface APIService1 {
    @Query(path = "/get?id=${id}")
    Flowable<String> get(Map<String, String> params, @PathParam("id") String ids, @PathParam int name);

    @Query(method = POST,url = "http://www.example.com/app/open")
    Flowable<Person> post(Map<String, String> params);

    @Query(method = POST,url = "http://www.example.com/app/open")
    Flowable<Person> post2(@QueryParam String query1);
}

```
执行build后会根据定义的接口生成调用类tinyritro.java
调用
```java
        TinyRitro build = new TinyRitro.Builder().client(OkHttpClientMgr.getInstance()).build();
        HashMap<String, String> params = new HashMap<>();
        params.put("aaa", "bbb");
        build.getAPIService1().get(params, "233",233)
                .compose(RxSchedulers.<String>io_main())
                .subscribe(new Consumer<String>() {
                    @Override
                    public void accept(String strings) throws Exception {
                        Log.d("result", "" + strings);
                    }
                }, new Consumer<Throwable>() {
                    @Override
                    public void accept(Throwable throwable) throws Exception {
                        throwable.printStackTrace();
                    }
                });
```


