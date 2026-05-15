smell_id:data_manipulation_by_migration

# Example 07

```elixir
defmodule Blog.Repo.Migrations.AddBodyHtmlToPosts do
  use Ecto.Migration

  import Ecto.Query
  alias Blog.Content.Post
  alias Blog.Repo

  def change do
    alter table(:posts) do
      add :body_html, :text
    end

    from(p in Post, select: %{id: p.id, body_markdown: p.body_markdown})
    |> Repo.all()
    |> Enum.each(fn %{id: id, body_markdown: markdown} ->
      html = Earmark.as_html!(markdown)

      from(p in Post, where: p.id == ^id)
      |> Repo.update_all(set: [body_html: html])
    end)
  end
end
```

The migration not only changes the table, but also runs content rendering logic to transform all pre-existing rows.
