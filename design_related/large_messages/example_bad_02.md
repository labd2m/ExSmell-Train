smell_id:large_messages
example_id:example_bad_02

# Example 02 - Image pipeline sends a huge binary to a worker process

```elixir
defmodule ThumbnailWorker do
  def start do
    spawn(__MODULE__, :loop, [])
  end

  def loop do
    receive do
      {:resize, image_binary, opts} ->
        # imagine image_binary is a 40 MB JPEG
        :erlang.byte_size(image_binary)
        opts[:format]
        loop()
    end
  end
end

defmodule ImagePipeline do
  def resize(path, worker_pid) do
    image_binary = File.read!(path)

    # Bad smell: a very large binary is sent repeatedly between processes
    send(worker_pid, {:resize, image_binary, format: :png})
  end
end

# Use example:
worker = ThumbnailWorker.start()
ImagePipeline.resize("full_resolution_photo.jpg", worker)
```

A large image binary is loaded in one process and then copied in a message to another process.
