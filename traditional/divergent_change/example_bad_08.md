smell_id:divergent_change

# Example 08

```elixir
defmodule Forum.Admin do
  @moduledoc """
  This module changes for moderation policy, badge rules,
  spam filtering, and digest email formatting.
  """

  # Reason to change: moderation policy
  def should_ban?(user) do
    user.report_count > 10 or user.last_violation == :hate_speech
  end

  # Reason to change: badge awarding rules
  def badge_for(post_count, likes_received) do
    cond do
      post_count > 500 and likes_received > 2_000 -> :legend
      post_count > 100 -> :contributor
      true -> :newcomer
    end
  end

  # Reason to change: spam heuristics
  def spam_score(post) do
    links = Regex.scan(~r/https?:\/\//, post.body) |> length()
    banned_terms = Enum.count(["crypto", "casino", "free money"], &String.contains?(post.body, &1))
    links * 10 + banned_terms * 20
  end

  # Reason to change: email digest content
  def digest_title(date) do
    "Forum digest for #{Date.to_iso8601(date)}"
  end
end
```
