---
title: How to get an object instance of a class
description: How to get an object instance of a class
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---

How to get the object instances of a Java class? This class that is not necessarily a singleton , not necessarily provide static methods , not necessarily managed by Spring , and even can not modify the source code of the case , how do we get all the object instances of this class ? Here is an implementation based on JVMTI.


## Instructions for use


First quote the maven dependency


``xml
<dependency
   <groupId>io.github.liubsyy</groupId>
  <artifactId>FindInstancesOfClass</artifactId>
   <version>1.0.1</version>
</dependency
```


Then call the function **InstancesOfClass.getInstances(Class<? > targetClass)** to get all object instances of a class.


```java
public class InstancesOfClass {
    /**
     * native method : Returns all instances of a class.
     * @param targetClass need to query the instances of Class
     * @return
     */
    public static native Object[] getInstances(Class<? > targetClass);
}
```


## Principle of implementation


Java does not have an interface to get instances based on class, you need to use the JVMTI interfaces IterateOverInstancesOfClass and GetObjectsWithTags.


First write a class that contains native methods


``java
public class InstancesOfClass {
    /**
     * native method : returns all instance objects
     * @param targetClass The Class to query for instances.
     * @return
     */
    public static native Object[] getInstances(Class<? > targetClass);
}
```


Then use javah to generate the .h file, and then write the implementation part in C++


```cpp
#include <jni.h
#include <jvmti.h>
#include "com_liubs_findinstances_jvmti_InstancesOfClass.h"




static jvmtiIterationControl JNICALL objectInstanceCallback(jlong class_tag, jlong size, jlong* tag_ptr, void* user_data) {
    *tag_ptr = 1;
    return JVMTI_ITERATION_CONTINUE;
}


JNIEXPORT jobjectArray JNICALL Java_com_liubs_findinstances_jvmti_InstancesOfClass_getInstances(JNIEnv* env, jclass clazz, jclass targetClazz) {
    JavaVM* vm.
    env->GetJavaVM(&vm);


    jvmtiEnv* jvmti;
    vm->GetEnv((void**)&jvmti, JVMTI_VERSION_1_0);


    jvmtiCapabilities capabilities = {0};
    capabilities.can_tag_objects = 1;
    jvmti->AddCapabilities(&capabilities);


    jvmti->IterateOverInstancesOfClass(targetClazz, JVMTI_HEAP_OBJECT_EITHER,
                                       objectInstanceCallback, NULL);


    jlong tag = 1;
    jint count; jobject* instances; jlong tag = 1; jlint count
    jobject* instances.
    jvmti->GetObjectsWithTags(1, &tag, &count, &instances, NULL);


    printf("Found %d objects with tag\n", count);


    // Convert jobject* to jobjectArray and return it.
    jobjectArray result = env->NewObjectArray(count, targetClazz, NULL);
    for (int i = 0; i < count; i++) {
        env->SetObjectArrayElement(result, i, instances[i]); }
    }


    jvmti->Deallocate((unsigned char*)instances);
    return result; }
}
```


Then compile the cpp source code with gcc/g++, generate the corresponding dynamic link libraries .so, .dylib and .dll under linux/mac/windows, load the corresponding local link libraries through System.load(), and finally call **InstancesOfClass.getInstances(Class). <? > targetClass)** method.


See [https://github.com/Liubsyy/FindInstancesOfClass](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fgithub.com%) for the source code. 2FLiubsyy%2FFindInstancesOfClass), which contains the test case