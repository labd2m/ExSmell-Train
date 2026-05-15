smell_id:complex_branching

# Example 06 — File upload service branching on many storage outcomes

```elixir
defmodule MyApp.Media do
  alias ExAws.S3

  def upload_avatar(user_id, local_path) do
    case File.read(local_path) do
      {:ok, binary} ->
        case S3.put_object("avatars", "users/#{user_id}.png", binary) |> ExAws.request() do
          {:ok, %{status_code: 200}} ->
            {:ok, "users/#{user_id}.png"}

          {:ok, %{status_code: 204}} ->
            {:ok, "users/#{user_id}.png"}

          {:ok, %{status_code: 400, body: body}} ->
            {:error, {:bad_request, body}}

          {:ok, %{status_code: 403}} ->
            {:error, :forbidden}

          {:ok, %{status_code: 404}} ->
            {:error, :bucket_not_found}

          {:ok, %{status_code: 409}} ->
            {:error, :conflict}

          {:ok, %{status_code: 413}} ->
            {:error, :payload_too_large}

          {:ok, %{status_code: 415}} ->
            {:error, :unsupported_media_type}

          {:ok, %{status_code: 500}} ->
            {:error, :storage_failure}

          {:ok, response} ->
            {:error, {:unexpected_storage_response, response}}

          {:error, {:http_error, 429, _}} ->
            {:error, :rate_limited}

          {:error, {:http_error, 503, _}} ->
            {:error, :service_unavailable}

          {:error, reason} ->
            {:error, {:storage_error, reason}}
        end

      {:error, :enoent} ->
        {:error, :file_not_found}

      {:error, :eacces} ->
        {:error, :permission_denied}

      {:error, reason} ->
        {:error, {:file_error, reason}}
    end
  end
end
```

This is a bad example because `upload_avatar/2` handles local file IO branches and remote storage branches together, making the function bulky and hard to test.
