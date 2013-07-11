---
author: Kent Chiu
published: true
layout: post
title: "Bean Validator 101(JSR 303)"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
categories:
  - java_ee
---




##### @Valid

當需要對field為其他複雜物件型別(Complex Object
Type)進行驗証時，可以透過`@Valid`.也可以對collection類別進行驗証。也就是說，可以對以下型別進行驗証：

1.  arrays
2.  implement java.lang.Iterable (especially Collection, List and Set)
3.  implement java.util.Map


```
    package javax.validation;
     
    import java.util.Set;
    import javax.validation.metadata.BeanDescriptor;
     
    public interface Validator {
        /**
         * Validates all constraints on object.
         */
        <T> Set<ConstraintViolation<T>> validate(T object, Class<?>... groups);
     
        /**
         * Validates all constraints placed on the property of object
         * named propertyName.
         */
        <T> Set<ConstraintViolation<T>> validateProperty(T object, String propertyName, Class<?>... groups);
     
        /**
         * Validates all constraints placed on the property named propertyName
         * of the class beanType would the property value be value
         */
        <T> Set<ConstraintViolation<T>> validateValue(Class<T> beanType, String propertyName, Object value, Class<?>... groups);
     
        /**
         * Return the descriptor object describing bean constraints.
         * The returned object (and associated objects including
         * ConstraintDescriptors) are immutable.
         */
        BeanDescriptor getConstraintsForClass(Class<?> clazz);
     
        /**
         * Return an instance of the specified type allowing access to
         * provider-specific APIs.  If the Bean Validation provider
         * implementation does not support the specified class,
         * ValidationException is thrown.
         */
        public <T> T unwrap(Class<T> type);
    }
```

### Validating groups

驗証群組功能，驗証郡組是可以就多個Entities的某些規則郡組化(grouping)並針對group**s**做驗証

### Payload

用來夾帶(attch)客制的metadata。

[java
