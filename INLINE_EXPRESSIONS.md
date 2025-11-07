# Browser-Executable Inline Expressions for MyST

This setup allows inline expressions to be evaluated in the browser (client-side) rather than just at build time.

## How It Works

1. **Code cells output variable data**: Code cells that define variables output them as JSON in a `<script>` tag
2. **JavaScript reads the data**: The `inline-eval.js` script extracts variable values from these script tags
3. **Expressions are evaluated**: The script processes `{eval}` expressions in markdown and replaces them with actual values

## Setup

1. **Copy the JavaScript file**: Ensure `inline-eval.js` is accessible in your built site. You can:
   - Place it in a `_static/` directory (if using Jupyter Book)
   - Or include it directly in your footer (as done in `footer.md`)

2. **Update your code cells**: Use the pattern shown in `01_sound_waves.ipynb` Cell 7:
   ```python
   from IPython.display import HTML
   import json
   
   var_data = {'f': f, 'A': A, 'T': float(T)}
   html_output = f"""
   <script type="application/json" id="wave-params">
   {json.dumps(var_data)}
   </script>
   """
   display(HTML(html_output))
   ```

3. **Use inline expressions**: In markdown cells, you can use:
   ```markdown
   Frequency: {eval}`f` Hz
   ```

## Limitations

- Only works for simple expressions (variables, basic arithmetic, `round()` function)
- Variables must be output as JSON from code cells
- Requires the JavaScript file to be loaded in the browser

## Alternative: Build-Time Evaluation

For simpler use cases, you can just use code cells that print formatted output (as shown in Cell 7), which works in the browser without any JavaScript.

