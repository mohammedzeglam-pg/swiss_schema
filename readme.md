# SwissSchema

[![Licensed under Apache 2.0](https://img.shields.io/hexpm/l/swiss_schema)](license)
[![Hex version](https://img.shields.io/hexpm/v/swiss_schema)](https://hex.pm/packages/swiss_schema)
[![Quality status](https://github.com/joeljuca/swiss_schema/actions/workflows/quality.yml/badge.svg)](https://github.com/joeljuca/swiss_schema/actions/workflows/quality.yml)

**A Swiss Army knife for your Ecto schemas**

SwissSchema is a query toolkit for Ecto schemas. It makes it easy to manipulate data using Ecto schemas by implementing relevant Ecto.Repo [Query API](https://hexdocs.pm/ecto/Ecto.Repo.html#query-api) and [Schema API](https://hexdocs.pm/ecto/Ecto.Repo.html#schema-api) functions, pre-configured to work specifically with the given Ecto schema.

## Setup

Add `swiss_schema` as a dependency in `mix.exs`:

```elixir
def deps do
  [
    # ...
    {:swiss_schema, "~> 0.4"}
  ]
end
```

Then, `use SwissSchema` in your Ecto schemas:

```elixir
# lib/my_app/accounts/user.ex

defmodule MyApp.Accounts.User do
  use Ecto.Schema
  use SwissSchema, repo: MyApp.Repo
end
```

That's it, you should be good to go.

## Usage

When you `use SwissSchema`, a collection of pre-configured functions will be added to your Ecto schema module. The functions are equivalent to two important Ecto.Repo APIs: the [Query API](https://hexdocs.pm/ecto/Ecto.Repo.html#query-api) and the [Schema API](https://hexdocs.pm/ecto/Ecto.Repo.html#schema-api).

```elixir
iex> User.get(1)
{:ok, %User{id: 1, ...}}

iex> User.get_by(email: "john@smiths.net")
{:ok, %User{id: 2, email: "john@smiths.net", ...}}
```

The motivation to have such API directly in your Ecto schema is to make function calls more idiomatic.

[Check the docs](https://hexdocs.pm/swiss_schema) for a complete list of available functions.

## Why?

If you find yourself asking what is the motivation for the creation of this module, this paragraph is for you. So, Elixir and Phoenix have this organizational concept called contexts, which is similar to the [Facade pattern](https://en.wikipedia.org/wiki/Facade_pattern): context modules should expose APIs for inner functionality to other modules. The thing is: implementing context modules require some effort, a good understanding level of the domain, etc., so you can break it down into smaller contexts that talk to each other through established APIs.

Well, it's not always that easy. Understanding your domain without diving into it is almost impossible, and you'll most probably acquire a better understanding of your domain and the system you're building after you start building it.

SwissSchema allows you to start building your system without having to set context boundaries. You can just generate a Ecto schema, `use SwissSchema` in it, and start manipulating data right away. And when time comes for you to turn your operations into real transactions, you slice your system into contexts, implement context modules, and start replacing SwissSchema function calls with your context functions.

Also, `User.get(1)` is just easier to read and understand than `Repo.get(User, 1)`. :)

## License

[Apache License 2.0](license)
