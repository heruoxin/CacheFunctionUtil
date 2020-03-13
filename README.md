# CacheFunctionUtil

([Chinese](README-CN.md))

A tiny helper class to provide cache for the result of repeated functions.

## How to use?

Let's take an example:

```
    public static List<String> getInstalledAppPackageNameList(Context context) {
        return CacheFunctionUtil.get().staticCache(() -> {
            return context.getPackageManager().getInstalledApplications(0).stream()
                    .map(applicationInfo -> applicationInfo.packageName)
                    .collect(Collectors.toList());
        });
    }
```

Then the `getInstalledAppPackageNameList` method will only run at the first time. The cached results will be directly obtained when repeatedly called.

### With paramenters?

```
    public static Palette generatePaletteFromDrawable(Drawable drawable) {
        return CacheFunctionUtil.get(R.id.function_generate_colors).staticCache(() -> {
            Bitmap bitmap = BitmapUtil.drawableToBitmap(drawable);
            return Palette.from(bitmap).generate();
        }, drawable);
    }
```

For different `drawable` object (identify by `hashCode()`), `CacheFunctionUtil` will catch different caches.

### Clear caches?

Just call `return CacheFunctionUtil.get(R.id.function_xxx_yyy).clear()`. 

It is recommended to assign different IDs to different logic parts.

## Dependence?

CacheFunctionUtil is only [one class](CacheFunctionUtil.java). Just CTRL+A, CTRL+C to your project.

## How it works?

It is just a HashMap and the point is lambda expression. On Android each lambda expression will be generated to a alone class at compile time so we can just use `lambda.getClass().getName() + paramenters.hashCode()` as key to store every result to a HashMap.

## License

MIT

