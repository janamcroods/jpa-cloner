
# JPA cloner #

The project allows cloning of JPA **entity subgraphs**. Entity subgraphs are defined by string patterns.
Cloned entities will have all basic properties copied by default.
Advanced control over the cloning process is supported via the **PropertyFilter** interface.

```xml
<repositories>
    <repository>
        <id>jitpack.io</id>
        <url>https://jitpack.io</url>
    </repository>
</repositories>
```



```xml
<dependency>
    <groupId>com.github.janamcroods</groupId>
    <artifactId>jpa-cloner</artifactId>
    <version>v1.0.3</version>
</dependency>
```

## Example usage
```java
Company company = entityManager.find(Company.class, companyId);
Company clone1 = JpaCloner.clone(company, "departments.*");
Company clone2 = JpaCloner.clone(company, "depa??ments");
Company clone3 = JpaCloner.clone(company, "departments+.(boss|employees).address");
// do not clone @Id and @Transient fields for the whole entity subgraph:
PropertyFilter filter = PropertyFilters.getAnnotationFilter(Id.class, Transient.class);
Company clone4 = JpaCloner.clone(company, filter, "*+");
// custom property filter
PropertyFilter myFilter = new PropertyFilter() {
    public boolean test(Object entity, String property) {
        return !"ignoredProperty".equals(property);
    }
};
Company clone5 = JpaCloner.clone(company, myFilter, "*+");
```

## Operators
- Dot "." separates paths: A.B.C
- Plus "+" generates at least one preceding path: A.B+.C
- Split "|" divides the path into two ways: A.(B|C).D
- Terminator "$" ends the preceding path: A.(B$|C).D
- Parentheses "(", ")" groups the paths.
- Wildcards "\*", "?" in property names: dumm?.pro\*ties

## Requirements
- The JPA cloner is tested only against **Hibernate**.
- Cloned entities must **correctly** implement equals() and hashCode().

Please refer to the **JpaCloner** class for more description.
