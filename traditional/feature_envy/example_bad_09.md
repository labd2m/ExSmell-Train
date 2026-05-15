smell_id:feature_envy

# Example 09

```elixir
defmodule BlogPost do
  def get(slug) do
    %{
      slug: slug,
      title: "Elixir Code Smells",
      body: "feature envy feature envy feature envy",
      published: true,
      tags: ["elixir", "design", "refactoring"]
    }
  end

  def word_count(post) do
    post.body
    |> String.split()
    |> length()
  end
end

defmodule BlogSEO do
  def meta_description(slug) do
    post = BlogPost.get(slug)
    words = BlogPost.word_count(post)
    tag_string = Enum.join(post.tags, ", ")

    "#{post.title} (#{words} words) - tags: #{tag_string}"
  end
end
```
