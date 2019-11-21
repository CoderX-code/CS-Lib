PropertyUtils 是 Apache commons 包中的一个工具类，用于动态获取和修改 bean 的值。

```Java
import java.util.Collections;
import java.util.Date;
import java.util.List;
import java.util.Map;

import org.apache.commons.lang3.builder.ToStringBuilder;
import org.springframework.stereotype.Component;

/**
 * @author lifeng
 * @since 2019-11-14 17:24
 */
@Component
public class Employee {
    private String name = "David";
    private int age = 22;
    private Date birth = new Date();
    private Map<String, String> position = Collections.emptyMap();
    private boolean managerFlag = false;
    private List<String> projects = Collections.emptyList();

    public Employee() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Date getBirth() {
        return birth;
    }

    public void setBirth(Date birth) {
        this.birth = birth;
    }

    public boolean isManagerFlag() {
        return managerFlag;
    }

    public void setManagerFlag(boolean managerFlag) {
        this.managerFlag = managerFlag;
    }

    public List<String> getProjects() {
        return projects;
    }

    public void setProjects(List<String> projects) {
        this.projects = projects;
    }

    public Map<String, String> getPosition() {
        return position;
    }

    public void setPosition(Map<String, String> position) {
        this.position = position;
    }

    @Override
    public String toString() {
        return new ToStringBuilder(this)
            .append("name", name)
            .append("age", age)
            .append("birth", birth)
            .append("position", position)
            .append("managerFlag", managerFlag)
            .append("projects", projects)
            .toString();
    }
}
```

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.commons.lang3.builder.ToStringBuilder;

/**
 * @author lifeng
 * @since 2019-11-14 20:27
 */
public class Company {
    private List<Employee> employeeList = new ArrayList<>(0);
    private Map<String, Employee> managersMap = new HashMap<>(0);

    public Company() {
    }

    public List<Employee> getEmployeeList() {
        return employeeList;
    }

    public void setEmployeeList(List<Employee> employeeList) {
        this.employeeList = employeeList;
    }

    public Map<String, Employee> getManagersMap() {
        return managersMap;
    }

    public void setManagersMap(Map<String, Employee> managersMap) {
        this.managersMap = managersMap;
    }

    @Override
    public String toString() {
        return new ToStringBuilder(this)
            .append("employeeList", employeeList)
            .append("managersMap", managersMap)
            .toString();
    }
}
```

PropertyUtils使用代码如下：

```Java
/*
 * Cainiao.com Inc.
 * Copyright (c) 2013-2019 All Rights Reserved.
 */
package com.codex.springbootdemo.Bean;

import java.lang.reflect.InvocationTargetException;
import java.util.ArrayList;
import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.commons.beanutils.PropertyUtils;

/**
 * @author lifeng
 * @since 2019-11-14 17:29
 */
public class PropertyUtilsTest {

    public static void main(String[] args)
        throws IllegalAccessException, NoSuchMethodException, InvocationTargetException {
        Employee employee = new Employee();
        System.out.println(employee.toString());

        // 通用方法
        PropertyUtils.setProperty(employee, "age", 24);
        System.out.println(PropertyUtils.getProperty(employee, "age"));

        // 简单类型
        PropertyUtils.setSimpleProperty(employee, "name", "Alice");
        PropertyUtils.setSimpleProperty(employee, "managerFlag", true);
        Date date = (Date)PropertyUtils.getProperty(employee, "birth");
        PropertyUtils.setSimpleProperty(employee, "birth", new Date(date.getTime() + 10000));
        System.out.println(employee.toString());

        // List类型
        List<String> projects = new ArrayList<>(3);
        projects.add("project1");
        projects.add("project2");
        projects.add("project3");
        PropertyUtils.setProperty(employee, "projects", projects);
        System.out.println(PropertyUtils.getProperty(employee, "projects"));
        PropertyUtils.setIndexedProperty(employee, "projects", 1, "123");
        System.out.println(PropertyUtils.getIndexedProperty(employee, "projects[1]"));
        System.out.println(PropertyUtils.getIndexedProperty(employee, "projects", 1));

        // Map类型
        Map<String, String> position = new HashMap<>(1);
        position.put("Dep1", "PE");
        PropertyUtils.setSimpleProperty(employee, "position", position);
        PropertyUtils.setMappedProperty(employee, "position", "Dep2", "PD");
        PropertyUtils.setMappedProperty(employee, "position(Dep3)", "PM");
        System.out.println(PropertyUtils.getMappedProperty(employee, "position(Dep2)"));
        System.out.println(PropertyUtils.getMappedProperty(employee, "position", "Dep3"));

        // 嵌套类型
        Company company = new Company();
        company.getEmployeeList().add(employee);
        company.getManagersMap().put("Dep1", employee);
        PropertyUtils.setNestedProperty(company, "employeeList[0].name", "Bob");
        System.out.println(company.toString());
        PropertyUtils.setNestedProperty(company, "managersMap(Dep1).name", "John");
        System.out.println(PropertyUtils.getNestedProperty(company, "employeeList[0].name"));
        System.out.println(PropertyUtils.getNestedProperty(company, "managersMap(Dep1).name"));
        System.out.println(company.toString());
    }
}
```

https://www.cnblogs.com/chenpi/p/6917499.html

