# Assetfile

The easiest way to export psd to favicon, WebClip and many others.

# Installation

1. Put `Rakefile` into your project.
2. Write your own Assetfile.
3. Run `bundle install`
3. Run `bundle exec rake asset:generate` to generate assets.

# Specimen

```yaml
assets/logo.psd:
  export:
    icons/favicon.ico:    16x16
    apple-touch-icon.png: 72x72

assets/mock.psd:
  slice: true
```