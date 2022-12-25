```ts
// todo/todoSchema.ts

export const getTodosSchema = z.object({
  query: z.object({
    limit: z.string().optional(),
    page: z.string().optional(),
  }),
});

export type GetTodos = z.infer<typeof getTodosSchema>;
```

```ts
// todo/todoController.ts

export const getTodos = async (req: GetTodos, res: Response) => {
  const page = req.query.page;
  const limit = req.query.limit;
  let todos;
  if (page && limit) {
    todos = await prisma.todo.findMany({
      skip: (parseInt(page) - 1) * parseInt(limit),
      take: parseInt(limit),
    });
  } else todos = await prisma.todo.findMany();
  return res.status(200).json(todos);
};
```
