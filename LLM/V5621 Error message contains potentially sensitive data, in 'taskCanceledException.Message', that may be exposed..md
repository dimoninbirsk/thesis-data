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
            "problem": "Unvalidated channel size configuration",
            "severity": "Medium",
            "cwe": "CWE-770: Missing Size Constraint for a Resource",
            "fix": "var engineChannel = Channel.CreateBounded<object>(new BoundedChannelOptions(Math.Min(2 * Configuration.EngineSettings.MaxDepth, 1000)) { SingleReader = true, SingleWriter = false, FullMode = BoundedChannelFullMode.DropOldest });",
            "explanation": "Dynamic configuration values should be validated to prevent resource exhaustion attacks"
        },
        {
            "type": "Smell",
            "problem": "Unclear task list composition",
            "severity": "Low",
            "cwe": null,
            "fix": "var tasks = new List<Task>\n{\n    Task.Run(() => writer.Run(cancellationToken)),\n    Task.Run(() => searcher.Run(cancellationToken)),\n    Task.Run(() => listener.Run(cancellationToken, args)),\n    Task.FromResult(uciChannel.Reader.Completion),\n    Task.FromResult(engineChannel.Reader.Completion)\n};",
            "explanation": "Mixing different task types in a single list can lead to confusion and maintenance issues"
        },
        {
            "type": "Optimization",
            "problem": "Inefficient exception handling",
            "severity": "Medium",
            "cwe": "CWE-704: Incorrect Damage Assessment",
            "fix": "try\n{\n    Console.WriteLine($\"{IdCommand.EngineName} {IdCommand.GetLynxVersion()} by {IdCommand.EngineAuthor}\");\n    await Task.WhenAll(tasks);\n}\ncatch (Exception e)\n{\n    _logger.Fatal(e, \"Unhandled exception: {Exception}\", e.Message);\n    Console.WriteLine(\"Unexpected exception: {0}\", e.Message);\n}\nfinally\n{\n    // cleanup\n}",
            "explanation": "Using Task.WhenAll and unified exception logging improves error handling and performance"
        },
        {
            "type": "Smell",
            "problem": "Unnecessary commented code",
            "severity": "Low",
            "cwe": null,
            "fix": "Remove commented line: //source.Cancel();",
            "explanation": "Dead code should be removed to prevent confusion and maintenance overhead"
        }
    ]
}

```

# GigaChat
```
{
  "findings": [
    {
      "type": "Smell",
      "problem": "Logger used inconsistently across exception types",
      "severity": "Medium",
      "cwe": "",
      "fix": "Consolidate logging approach",
      "explanation": "Inconsistent logging makes debugging harder."
    },
    {
      "type": "Optimization",
      "problem": "Channels closed without ensuring all readers have finished",
      "severity": "Medium",
      "cwe": "",
      "fix": "Synchronize channel completions properly",
      "explanation": "Incomplete synchronization could lead to lost messages or data corruption."
    }
  ]
}
```