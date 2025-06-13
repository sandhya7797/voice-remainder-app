# voice-remainder-app

# Issues

# 📝 Voice Remainder App – Key Learnings

1. **Infinite JSON Response in Postman**

    * Caused by bi-directional mapping in JPA.
    * Use `@JsonManagedReference` on the parent side and `@JsonBackReference` on the child side to prevent infinite recursion.

2. **Lombok Annotations Not Working**

    * Check if Lombok is properly added in `pom.xml`.
    * Ensure annotation processing is enabled in your IDE.
    * Watch out for dependencies that exclude Lombok.

3. **Primary Key Generation**

    * Use `@GeneratedValue(strategy = GenerationType.IDENTITY)` to let the database auto-generate the primary key for each new record.

4. **JPA Relationship Mapping**

    * In the parent entity (e.g., `Task`):
      `@OneToMany(mappedBy = "task", cascade = CascadeType.ALL)`
    * In the child entity (e.g., `TodoItem`):
      `@ManyToOne` + `@JoinColumn(name = "task_id")`
    * This creates a `task_id` column in the child table.

5. **Cascade and Orphan Removal**

    * Use `cascade = CascadeType.ALL` to propagate changes from parent to child.
    * Use `orphanRemoval = true` to delete child records when they are removed from the parent's list.

6. **Important Behavior of `orphanRemoval = true`**

    * It deletes child records from the database when they are no longer referenced by the parent entity.
    * Helpful when updating or replacing the full child list via PUT API.

7. **Difference Between `set()` and `clear() + addAll()`**

    * Using `.setTodoItemList(newList)` replaces the list reference, which Hibernate may not track properly.
    * Instead, use:

     
      task.getTodoItemList().clear();
      task.getTodoItemList().addAll(newList);
      

      This modifies the existing list and enables proper orphan removal.

---


