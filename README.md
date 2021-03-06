# Scrivener.Html [![Build Status](https://semaphoreci.com/api/v1/projects/3b1ad27c-8991-4208-94d0-0bae42108482/638637/badge.svg)](https://semaphoreci.com/mgwidmann/scrivener_html)

Helpers built to work with [Scrivener](https://github.com/drewolson/scrivener)'s page struct to easily build HTML output for various CSS frameworks.

## Setup

Add to `mix.exs`

```elixir
  defp deps do
    [
      # ...
      {:scrivener_html, "~> 1.1"}
      # ...
    ]
  end
```

For use with Phoenix.HTML, configure the `:routes_helper` module like the following:

```elixir
config :scrivener_html,
  routes_helper: MyApp.Router.Helpers
```

Import to you view.

```elixir
defmodule MyApp.UserView do
  use MyApp.Web, :view
  import Scrivener.HTML
end
```

## Example Usage

Use in your template.

```elixir
<%= pagination_links @conn, @page %>
```

Where `@page` is a `%Scrivener.Page{}` struct returned from `Repo.paginate/2`.

If your resource has any url parameters to be supplied, you should provide them as the 3rd parameter. For example, given a scope like:

```elixir
scope "/:locale", App do
  pipe_through [:browser]

  get "/page", PageController, :index
end
```

You would need to pass in the `:locale` parameter and `:path` option like so:

```elixir
<%= pagination_links @conn, @page, ["en"], path: &page_path/4 %>
```

Any additional query string parameters can be passed in as well.

```elixir
<%= pagination_links @conn, @page, ["en"], some_parameter: "data" %>
<%# OR IF NO URL PARAMETERS %>
<%= pagination_links @conn, @page, some_parameter: "data" %>
```

## Customizing Output

Below are the defaults which are used without passing in any options.

```elixir
<%= pagination_links @conn, @page, distance: 5, next: ">>", previous: "<<", first: true, last: true, view_style: :bootstrap %>
```

There are three view styles currently supported:

- `:bootstrap` (the default) This styles the pagination links in a manner that
  is expected by Bootstrap 3.x.
- `:foundation` This styles the pagination links in a manner that is expected
  by Foundation for Sites 6.x.
- `:semantic` This styles the pagination links in a manner that is expected by
  Semantic UI 2.x.

For custom HTML output, see `Scrivener.HTML.raw_pagination_links/2`.

See `Scrivener.HTML.raw_pagination_links/2` for option descriptions.

Scrivener.HTML can be included in your view and then just used with a simple call to `pagination_links/1`.

```elixir
iex> Scrivener.HTML.pagination_links(%Scrivener.Page{total_pages: 10, page_number: 5})
{:safe,
  ["<nav>",
    ["<ul class=\"pagination\">",
      [["<li>", ["<a class=\"\" href=\"?page=4\">", "&lt;&lt;", "</a>"], "</li>"],
      ["<li>", ["<a class=\"\" href=\"?page=1\">", "1", "</a>"], "</li>"],
      ["<li>", ["<a class=\"\" href=\"?page=2\">", "2", "</a>"], "</li>"],
      ["<li>", ["<a class=\"\" href=\"?page=3\">", "3", "</a>"], "</li>"],
      ["<li>", ["<a class=\"\" href=\"?page=4\">", "4", "</a>"], "</li>"],
      ["<li>", ["<a class=\"active\" href=\"?page=5\">", "5", "</a>"], "</li>"],
      ["<li>", ["<a class=\"\" href=\"?page=6\">", "6", "</a>"], "</li>"],
      ["<li>", ["<a class=\"\" href=\"?page=7\">", "7", "</a>"], "</li>"],
      ["<li>", ["<a class=\"\" href=\"?page=8\">", "8", "</a>"], "</li>"],
      ["<li>", ["<a class=\"\" href=\"?page=9\">", "9", "</a>"], "</li>"],
      ["<li>", ["<a class=\"\" href=\"?page=10\">", "10", "</a>"], "</li>"],
      ["<li>", ["<a class=\"\" href=\"?page=6\">", "&gt;&gt;", "</a>"], "</li>"]],
      "</ul>"], "</nav>"]}
```
