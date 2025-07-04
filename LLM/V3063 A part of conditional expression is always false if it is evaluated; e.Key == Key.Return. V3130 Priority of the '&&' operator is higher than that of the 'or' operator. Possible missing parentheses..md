V3022 Expression 'Text.Length == 10' is always false.

```
**Role:** Senior .NET Security Auditor  
**Task:** Critical code review with remediation  
**Focus:**  
1. Security vulnerabilities (map to CWE)  
2. Code smells 
3. Bugs 
4. Performance optimizations  

**Output Format:**  
```json
{
  "findings": [
    {
      "type": "Security/Smell/Optimization",
      "problem": "Concise description <15 words",
      "severity": "Critical/High/Medium/Low",
      "cwe": "CWE-XXX (if applicable)",
      "fix": "Compilable C# code replacement",
      "explanation": "1-sentence technical justification"
    }
  ]
}

**Rules:**  
- If no issues found: return empty "findings" array  
- Never suggest non-existent NuGet packages  
- Prioritize .NET best practices  
- Assume enterprise-scale deployment context

**Code**:
```
# YandexGpt
```
{
    "findings": [
        {
            "type": "Security",
            "problem": "Insecure handling of special keys without validation",
            "severity": "Medium",
            "cwe": "CWE-862",
            "fix": "if (e.Key == Key.Enter || e.Key == Key.Return || e.Key == Key.Tab || e.Key == Key.Left || e.Key == Key.Right || e.Key == Key.Escape || e.Key == Key.Home || e.Key == Key.End) {\n    e.Handled = true; // Changed to true for security\n    return;\n}",
            "explanation": "Unhandled keys can lead to unintended navigation or selection manipulation"
        },
        {
            "type": "Smell",
            "problem": "Redundant substring operations in condition",
            "severity": "Low",
            "cwe": null,
            "fix": "var selectedText = Text.Substring(SelectionStart, SelectionLength);\nvar separatorSelected = selectedText.Contains(':') || selectedText.Contains('.');",
            "explanation": "Duplicate substring calls can be optimized by storing result in variable"
        },
        {
            "type": "Optimization",
            "problem": "Repetitive string manipulation logic",
            "severity": "Medium",
            "cwe": null,
            "fix": "string UpdateTextWithSeparator()\n{\n    switch (Text.Length)\n    {\n        case 1:\n            return \"0\" + (Text.Equals(\"0\") ? \"1\" : Text) + \":\";\n        case 4:\n            return Text.Substring(0, 3) + \"0\" + (Text[3] == '0' ? \"1\" : Text[3].ToString()) + (DisplaySeconds ? \":\" : \"\");\n        case 7:\n            return Text.Substring(0, 6) + \"0\" + Text[6];\n        case 10:\n            return Text.Substring(0, 9) + Text[6].ToString().PadLeft(3, '0');\n        default:\n            return Text;\n    }\n}",
            "explanation": "Refactoring repetitive string logic into a reusable method improves maintainability"
        },
        {
            "type": "Smell",
            "problem": "Complex conditional logic for backspace/delete",
            "severity": "Medium",
            "cwe": null,
            "fix": "if (e.Key == Key.Back || e.Key == Key.Delete)\n{\n    if (ShouldClearText())\n    {\n        Text = \"\";\n        RaiseEvent(new RoutedEventArgs(TextChangedEvent));\n        UpdateSource();\n    }\n    e.Handled = false;\n    return;\n}\n\nprivate bool ShouldClearText()\n{\n    return SelectionLength == Text.Length ||\n           (Text.Length == 1 &&\n            ((SelectionStart == 0 && e.Key == Key.Delete) ||\n             (SelectionStart == 1 && e.Key == Key.Back)));\n}",
            "explanation": "Extracting complex condition into a method improves readability"
        }
    ]
}

```

# GigaChat
```
{
  "findings": [
    {
      "type": "Security",
      "problem": "Improper Input Validation",
      "severity": "High",
      "cwe": "CWE-20",
      "fix": "// Validate input before processing\nif (!Regex.IsMatch(e.Key.ToString(), \"^[a-zA-Z0-9]+$\"))\n{\ne.Handled = true;\nreturn;\n}",
      "explanation": "Input validation prevents injection attacks."
    },
    {
      "type": "Performance Optimization",
      "problem": "Inefficient String Manipulation",
      "severity": "Medium",
      "cwe": "",
      "fix": "// Use StringBuilder for efficient string manipulation\nvar sb = new StringBuilder();\nsb.Append(\"0\");\nif (Text.Equals(\"0\"))\n{\nsb.Append(\"1\");\n}\nelsb.Append(Text);",
      "explanation": "StringBuilder reduces memory allocations during concatenation."
    },
    {
      "type": "Code Smell",
      "problem": "Magic Numbers",
      "severity": "Low",
      "cwe": "",
      "fix": "// Define constants for magic numbers\nconst int MAX_LENGTH = 8;\nif (Text.Length > MAX_LENGTH)\n{\ne.Handled = true;\nreturn;\n}",
      "explanation": "Constants improve readability and maintainability."
    }
  ]
}
```
