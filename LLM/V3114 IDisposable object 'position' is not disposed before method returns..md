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
            "type": "Smell",
            "problem": "Excessive use of magic numbers and casts",
            "severity": "Medium",
            "cwe": null,
            "fix": "Replace casts with enum-based access:\nposition.PieceBitBoards[Piece.p].Print();\nAttacks.PawnAttacks[Side.White][BoardSquare.e4].Print();",
            "explanation": "Using enums directly improves code readability and maintainability"
        },
        {
            "type": "Smell",
            "problem": "Repetitive code for setting bits",
            "severity": "Medium",
            "cwe": null,
            "fix": "Create helper method:\nvoid SetPieces(Position position, Piece pieceType, params BoardSquare[] squares)\n{\n    foreach (var square in squares)\n    {\n        position.PieceBitBoards[pieceType].SetBit(square);\n    }\n}\n\n// Usage:\nSetPieces(position, Piece.q, BoardSquare.b7, BoardSquare.d7, BoardSquare.f7, BoardSquare.h7);",
            "explanation": "DRY principle violation leads to code duplication and maintenance issues"
        },
        {
            "type": "Smell",
            "problem": "Lack of method documentation",
            "severity": "Low",
            "cwe": null,
            "fix": "/// <summary>\n/// Demonstrates square attack detection logic for various chess pieces\n/// </summary>",
            "explanation": "Missing documentation makes code purpose unclear for future maintainers"
        },
        {
            "type": "Optimization",
            "problem": "Inefficient board initialization",
            "severity": "Medium",
            "cwe": null,
            "fix": "Refactor repeated Position creation:\nPosition CreatePosition(string fen)\n{\n    var position = new Position(fen);\n    return position;\n}\n\n// Usage:\nvar position = CreatePosition(Constants.EmptyBoardFEN);",
            "explanation": "Reduces code duplication and improves performance in high-throughput scenarios"
        },
        {
            "type": "Smell",
            "problem": "Hardcoded test cases",
            "severity": "Medium",
            "cwe": null,
            "fix": "Extract test cases into separate configuration:\nstatic readonly List<(string FEN, Piece PieceType, BoardSquare[] Squares)> TestCases = new()\n{\n    (Constants.EmptyBoardFEN, Piece.q, new[] { BoardSquare.b7, BoardSquare.d7 }),\n    // other test cases\n};",
            "explanation": "Hardcoded values make maintenance difficult and error-prone"
        }
    ]
}

```

# GigaChat:

```json
{
  "findings": [
    {
      "type": "Smell",
      "problem": "Method performs multiple unrelated tasks",
      "severity": "Medium",
      "cwe": "",
      "fix": "Refactor method into smaller focused methods",
      "explanation": "Single responsibility principle violation impacts maintainability."
    },
    {
      "type": "Performance Optimization",
      "problem": "Repeated object instantiations in loop-like structure",
      "severity": "Medium",
      "cwe": "",
      "fix": "Instantiate objects outside of repetitive block",
      "explanation": "Object creation overhead can be reduced by reusing instances."
    }
  ]
}
```

