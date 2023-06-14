# How to disable Pylance Errors

To disable specific Pylance errors or warnings, you can add special comments, called "type comments", to your code. These comments tell Pylance to ignore specific errors or warnings for the code that follows.

To disable a Pylance error or warning, follow these steps:

1. Find the error or warning you want to disable in the Pylance output or in the Problems panel in Visual Studio Code.

2. Hover over the error or warning to see the error code. For example, the error code for "Unused import" is `F401`.

3. Add a type comment to your code to disable the error or warning. The format of the type comment is `# type: ignore[error_code]`. Replace `error_code` with the code of the error or warning you want to disable. For example, to disable the "Unused import" error, add the following comment to the line with the unused import:

   ```python
   import unused_module  # type: ignore[F401]
   ```

4. Save your changes and Pylance should no longer report that specific error or warning.

You can also disable all Pylance errors and warnings for a specific file by adding the following comment at the beginning of the file:

```python
# pyright: reportUnusedImport=false, reportUnusedVariable=false
```

This disables both the "Unused import" and "Unused variable" warnings. You can add other configuration options to this comment to disable other Pylance features. See the Pylance documentation for more information on configuration options.

Note that while disabling specific errors or warnings can make your code easier to read or work around some issues, it is generally better to fix the underlying issues that are causing the errors or warnings.