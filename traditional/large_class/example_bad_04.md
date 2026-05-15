smell_id:large_class

# Example 04

```elixir
defmodule Blog do
  alias CMS.Mailer
  alias CMS.SearchIndexer

  # Post CRUD
  def create_post(attrs), do: {:ok, attrs}
  def update_post(post, attrs), do: {:ok, Map.merge(post, attrs)}
  def delete_post(post), do: {:ok, post}

  # Comment moderation
  def approve_comment(comment), do: %{comment | status: :approved}
  def reject_comment(comment), do: %{comment | status: :rejected}
  def spam_comment(comment), do: %{comment | status: :spam}

  # Tag management
  def add_tag(post, tag), do: Map.update(post, :tags, [tag], &[tag | &1])
  def remove_tag(post, tag), do: Map.update(post, :tags, [], &Enum.reject(&1, fn t -> t == tag end))

  # SEO
  def generate_slug(title) do
    title
    |> String.downcase()
    |> String.replace(" ", "-")
  end

  def meta_description(body) do
    body |> String.slice(0, 150)
  end

  # Search indexing
  def reindex(post) do
    SearchIndexer.push(post)
  end

  # Email notifications
  def notify_subscribers(post, emails) do
    Enum.each(emails, &Mailer.send(&1, "New post published: #{post.title}"))
  end
end
```
