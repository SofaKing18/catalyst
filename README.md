# Catalyst

Very basic WebDav client for Elixir. Uses native erlang :httpc library, does not require any additional dependencies

## Usage

```elixir
# Start a genserver process

Catalyst.start_link host: "http://example-webdav.com", user: "some_user", password: "123"

# or, in your OTP app add Catalyst as a worker

  def start(_type, _args) do
    import Supervisor.Spec

    webdav_conf = [host: "http://example-webdav.com", user: "some_user", password: "123"]
    # or you can supply just [host: "some_host", digest: "sadsadasd="]
    # explicitly supplied digest will override digest hashed from user:password
    # currently supports only Basic HTTP authentication
    children = [
      worker(Catalyst, [webdav_conf])
    ]

    Supervisor.start_link(children, strategy: :one_for_one)
  end
```

Authentication params will be stored inside genserver process, and you a good to go.

Supported methods

```elixir
# GET file from uri
Catalyst.get(uri)

# http HEAD request to uri
Catalyst.head(uri)

# Create or update file with data
Catalyst.put(uri, data)

# Upload file to uri
Catalyst.put_file(uri, filepath)

# Upload whole directory recursively to uri
Catalyst.put_directory(uri, dirpath)

# Delete file from uri
Catalyst.delete(uri)
```

## Installation

If [available in Hex](https://hex.pm/docs/publish), the package can be installed
by adding `catalyst` to your list of dependencies in `mix.exs`:

```elixir
def deps do
  [{:catalyst, "~> 0.1.0"}]
end
```

Documentation can be generated with [ExDoc](https://github.com/elixir-lang/ex_doc)
and published on [HexDocs](https://hexdocs.pm). Once published, the docs can
be found at [https://hexdocs.pm/catalyst](https://hexdocs.pm/catalyst).

