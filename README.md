# Nitro Docs

Technical specifiction of the Nitro margin trading protocol.

## Development

To run the book locally:

1. Install [Hugo](https://gohugo.io/).
1. Clone the repo:
   ```shell
   $ git clone https://github.com/NitroSwap/docs
   $ cd docs
   ```
1. Run:
   ```shell
   $ git submodule update --init
   $ hugo server -D
   ```

You can also run `hugo server --minify --theme hugo-book` instead.

1. Visit http://localhost:1313/ (or whatever URL the previous command outputs!)
