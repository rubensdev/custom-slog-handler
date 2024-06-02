# PrettyLog: A Custom Slog Handler

A custom slog handler following the tutorial of [@dustinmoris](https://x.com/dustinmoris) [Creating a pretty console logger using Go's slog package](https://dusted.codes/creating-a-pretty-console-logger-using-gos-slog-package). I will use this custom handle for development purposes and update it if necessary.

```bash
go get github.com/rubensdev/prettylog
```

```go
// Example with no *slog.HandlerOptions:
func example() {
    logger := slog.New(prettylog.NewHandler(nil))

    logger.Error("Whoops, something went wrong!", "error", errors.New("zero SLICES of pizza"))
}

// Create a child logger with an additional group of attributes attached to it:
func example2() {
    logger := slog.
        New(prettylog.NewHandler(nil)).
        With(slog.Group("status",
            "energy", "high",
            "mood", "amazing",
        ))
    logger.Error("Whoops, something went wrong!", "error", errors.New("i'm not in the context"))
}

// ReplaceAttr is supported:
func example3() {
    opts := &slog.HandlerOptions{
        Level:        slog.LevelDebug,
        AddSource:    false,
        ReplaceAttr:  func(groups []string, a slog.Attr) slog.Attr {
            if a.Key == "nothing" {
                return slog.Attr{}
            }
            return a
        }
    }
    logger := slog.New(prettylog.NewHandler(opts))

    logger = logger.WithGroup("data")

    logger.Info("いいですね",
        "count", 10,
        "pi", 3.14,
    )
}
```
