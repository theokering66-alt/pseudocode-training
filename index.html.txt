import React, { useMemo, useRef, useState } from "react";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Textarea } from "@/components/ui/textarea";
import { Badge } from "@/components/ui/badge";
import { Progress } from "@/components/ui/progress";
import { ScrollArea } from "@/components/ui/scroll-area";
import {
  CheckCircle2,
  HelpCircle,
  Lightbulb,
  RefreshCw,
  Sparkles,
  Trophy,
  Code2,
  ChevronRight,
  BookOpen,
  GraduationCap,
  ClipboardList,
  BrainCircuit,
  Play,
  TerminalSquare,
} from "lucide-react";

const questionBank = [
  {
    id: 1,
    topic: "Loops and totals",
    level: "Beginner",
    marks: 8,
    concept: "Using a loop to repeatedly input values and build a running total.",
    steps: [
      "DECLARE the variables you need.",
      "Set Total to 0 before starting the loop.",
      "Use a loop to INPUT 5 numbers.",
      "Add each number to Total inside the loop.",
      "OUTPUT Total at the end.",
    ],
    question: `Write pseudocode to:
1. Input 5 numbers
2. Find the total
3. Output the total`,
    keyChecks: [
      { label: "Takes input", marks: 1, patterns: ["input", "read"] },
      { label: "Uses a loop", marks: 1, patterns: ["for", "while", "repeat"] },
      { label: "Initialises total", marks: 1, patterns: ["total ← 0", "total <- 0", "total = 0", "total=0"] },
      { label: "Adds number to total", marks: 2, patterns: ["total ← total +", "total <- total +", "total = total +"] },
      { label: "Outputs total", marks: 1, patterns: ["output total", "print total", "display total"] },
      { label: "Uses exam style structure", marks: 2, patterns: ["declare", "next", "endwhile", "until"] },
    ],
    hints: [
      "Start by setting TOTAL to 0 before the loop.",
      "Inside the loop, INPUT one number each time.",
      "After input, add that number to TOTAL.",
      "At the end, OUTPUT TOTAL.",
    ],
    modelAnswer: `DECLARE Number : INTEGER
DECLARE Total : INTEGER
Total ← 0
FOR Counter ← 1 TO 5
    INPUT Number
    Total ← Total + Number
NEXT Counter
OUTPUT Total`,
  },
  {
    id: 2,
    topic: "Selection",
    level: "Beginner",
    marks: 8,
    concept: "Using IF, ELSE and ENDIF to make a decision.",
    steps: [
      "INPUT the value first.",
      "Use IF to test the condition.",
      "For even numbers, use MOD 2.",
      "Use ELSE for the other result.",
      "Close the structure with ENDIF.",
    ],
    question: `Write pseudocode to:
1. Input a number
2. Output "Even" if it is even
3. Otherwise output "Odd"`,
    keyChecks: [
      { label: "Takes input", marks: 1, patterns: ["input", "read"] },
      { label: "Uses IF statement", marks: 1, patterns: ["if"] },
      { label: "Checks for even using MOD", marks: 2, patterns: ["mod", "mod 2"] },
      { label: "Outputs Even", marks: 1, patterns: ['output "even"', 'print "even"', 'display "even"', "output even"] },
      { label: "Outputs Odd", marks: 1, patterns: ['output "odd"', 'print "odd"', 'display "odd"', "output odd"] },
      { label: "Closes IF properly", marks: 2, patterns: ["then", "else", "endif"] },
    ],
    hints: [
      "Use Number MOD 2.",
      "If the remainder is 0, the number is even.",
      "Remember ELSE and ENDIF.",
    ],
    modelAnswer: `DECLARE Number : INTEGER
INPUT Number
IF Number MOD 2 = 0 THEN
    OUTPUT "Even"
ELSE
    OUTPUT "Odd"
ENDIF`,
  },
  {
    id: 3,
    topic: "Arrays",
    level: "Intermediate",
    marks: 10,
    concept: "Using an array to store values and compare them one by one.",
    steps: [
      "DECLARE the array and Largest.",
      "Use a loop to INPUT values into the array.",
      "Set Largest to the first array value.",
      "Loop through the remaining values.",
      "Update Largest when a bigger value is found.",
      "OUTPUT Largest.",
    ],
    question: `Write pseudocode to:
1. Input 5 numbers into an array
2. Find the largest number
3. Output the largest number`,
    keyChecks: [
      { label: "Declares array or indexed values", marks: 1, patterns: ["array", "["] },
      { label: "Uses input", marks: 1, patterns: ["input", "read"] },
      { label: "Uses loop for 5 values", marks: 2, patterns: ["for", "while", "repeat"] },
      { label: "Initialises largest", marks: 2, patterns: ["largest ←", "largest <-", "largest ="] },
      { label: "Compares current value with largest", marks: 2, patterns: ["> largest", "if"] },
      { label: "Updates largest", marks: 1, patterns: ["largest ←", "largest <-", "largest ="] },
      { label: "Outputs largest", marks: 1, patterns: ["output largest", "print largest", "display largest"] },
    ],
    hints: [
      "Store the 5 values first.",
      "Set Largest to the first array value before comparing the rest.",
      "Use IF to update Largest when a bigger value is found.",
    ],
    modelAnswer: `DECLARE Number : ARRAY[1:5] OF INTEGER
DECLARE Largest : INTEGER
FOR Counter ← 1 TO 5
    INPUT Number[Counter]
NEXT Counter
Largest ← Number[1]
FOR Counter ← 2 TO 5
    IF Number[Counter] > Largest THEN
        Largest ← Number[Counter]
    ENDIF
NEXT Counter
OUTPUT Largest`,
  },
  {
    id: 4,
    topic: "Validation",
    level: "Intermediate",
    marks: 10,
    concept: "Repeating input until valid data is entered.",
    steps: [
      "INPUT a value inside a loop.",
      "Check whether the value is within the allowed range.",
      "Keep repeating until the value is valid.",
      "OUTPUT the final message after the loop.",
    ],
    question: `Write pseudocode to keep inputting a number until the user enters a value between 1 and 10 inclusive. Then output "Accepted".`,
    keyChecks: [
      { label: "Uses input", marks: 1, patterns: ["input", "read"] },
      { label: "Uses repetition", marks: 2, patterns: ["repeat", "until", "while", "endwhile"] },
      { label: "Checks lower bound", marks: 2, patterns: [">= 1", ">=1", "< 1"] },
      { label: "Checks upper bound", marks: 2, patterns: ["<= 10", "<=10", "> 10"] },
      { label: "Stops when valid", marks: 2, patterns: ["until", "while"] },
      { label: "Outputs Accepted", marks: 1, patterns: ["accepted"] },
    ],
    hints: [
      "A REPEAT UNTIL loop works well here.",
      "The number is valid only when it is from 1 to 10 inclusive.",
      "You can also use a WHILE loop if you write the condition correctly.",
    ],
    modelAnswer: `DECLARE Number : INTEGER
REPEAT
    INPUT Number
UNTIL Number >= 1 AND Number <= 10
OUTPUT "Accepted"`,
  },
  {
    id: 5,
    topic: "Searching",
    level: "Advanced",
    marks: 12,
    concept: "Searching through an array using a flag variable to track whether a match is found.",
    steps: [
      "DECLARE the array, search value and Found flag.",
      "Set Found to FALSE before searching.",
      "INPUT all array values.",
      "INPUT the search value.",
      "Loop through the array and compare each value.",
      "Set Found to TRUE if a match is found.",
      "OUTPUT Found or Not Found at the end.",
    ],
    question: `Write pseudocode to:
1. Input 10 numbers into an array
2. Input a search value
3. Output "Found" if the value exists in the array
4. Otherwise output "Not Found"`,
    keyChecks: [
      { label: "Uses array", marks: 1, patterns: ["array", "["] },
      { label: "Inputs 10 values", marks: 2, patterns: ["for", "1 to 10", "1 TO 10"] },
      { label: "Inputs search value", marks: 1, patterns: ["search", "target"] },
      { label: "Uses a flag or found variable", marks: 2, patterns: ["found", "flag"] },
      { label: "Searches through array", marks: 2, patterns: ["if", "number[", "="] },
      { label: "Sets flag when matched", marks: 2, patterns: ["found ← true", "found <- true", "found = true"] },
      { label: "Outputs Found or Not Found", marks: 2, patterns: ["found", "not found"] },
    ],
    hints: [
      "Use a Boolean variable like Found.",
      "Set Found to FALSE before the loop.",
      "If a match is found, change Found to TRUE.",
      "At the end, output based on the value of Found.",
    ],
    modelAnswer: `DECLARE Number : ARRAY[1:10] OF INTEGER
DECLARE SearchValue : INTEGER
DECLARE Found : BOOLEAN
Found ← FALSE
FOR Counter ← 1 TO 10
    INPUT Number[Counter]
NEXT Counter
INPUT SearchValue
FOR Counter ← 1 TO 10
    IF Number[Counter] = SearchValue THEN
        Found ← TRUE
    ENDIF
NEXT Counter
IF Found = TRUE THEN
    OUTPUT "Found"
ELSE
    OUTPUT "Not Found"
ENDIF`,
  },
];

const snippets = [
  "DECLARE ",
  "INPUT ",
  "OUTPUT ",
  "IF  THEN\n    \nENDIF",
  "ELSE",
  "FOR Counter ← 1 TO \nNEXT Counter",
  "WHILE  DO\n    \nENDWHILE",
  "REPEAT\n    \nUNTIL ",
  "ARRAY[1:5] OF INTEGER",
  "MOD",
  "AND",
  "OR",
  "TRUE",
  "FALSE",
  "←",
];

const lessons = [
  {
    id: "basics",
    title: "Pseudocode basics",
    summary: "Learn the main IGCSE pseudocode words and what they do.",
    blocks: [
      { heading: "Input and output", body: "Use INPUT to accept data from the user and OUTPUT to display a result." },
      { heading: "Variables", body: "Use DECLARE to show the name and type of a variable. Then use ← to store a value." },
      { heading: "Exam style", body: "IGCSE pseudocode usually uses clear keywords like IF, THEN, ELSE, ENDIF, FOR, NEXT, WHILE and ENDWHILE." },
    ],
  },
  {
    id: "selection",
    title: "Selection and decisions",
    summary: "Use IF statements to choose between two or more actions.",
    blocks: [
      { heading: "IF structure", body: "A basic decision uses IF condition THEN followed by the action. Use ELSE for the other choice and ENDIF to close it." },
      { heading: "Comparison", body: "Conditions often use =, >, <, >= or <=. For even numbers, use MOD 2." },
      { heading: "Common mistake", body: "Do not forget ENDIF. In exam answers, clear structure helps you gain marks." },
    ],
  },
  {
    id: "loops",
    title: "Loops and repetition",
    summary: "Learn when to use FOR, WHILE and REPEAT UNTIL.",
    blocks: [
      { heading: "FOR loop", body: "Use FOR when you know exactly how many times something repeats." },
      { heading: "WHILE loop", body: "Use WHILE when the loop should keep going while a condition is true." },
      { heading: "REPEAT UNTIL", body: "Use REPEAT UNTIL when you want the loop body to run at least once before checking the condition." },
    ],
  },
  {
    id: "arrays",
    title: "Arrays and searching",
    summary: "Store many values and work through them one by one.",
    blocks: [
      { heading: "Arrays", body: "An array stores multiple values of the same type, for example ARRAY[1:5] OF INTEGER." },
      { heading: "Index", body: "Each position in the array has an index, such as Number[1] or Number[5]." },
      { heading: "Searching", body: "To search an array, compare each item with the search value and use a Boolean flag if needed." },
    ],
  },
];

function normalize(text) {
  return text.toLowerCase().replace(/\s+/g, " ").trim();
}

function includesAny(code, patterns, strictness = "Medium") {
  const text = normalize(code);
  const exact = patterns.some((pattern) => text.includes(pattern.toLowerCase()));
  if (exact) return true;

  if (strictness === "Easy") {
    return patterns.some((pattern) => {
      const token = pattern.toLowerCase().split(" ")[0].replace(/[^a-z0-9\[\]<>==]/g, "");
      return token && text.includes(token);
    });
  }

  if (strictness === "Medium") {
    return patterns.some((pattern) => {
      const words = pattern.toLowerCase().split(" ").filter(Boolean);
      return words.length > 0 && words.some((word) => word.length > 2 && text.includes(word));
    });
  }

  return false;
}

function detectIssues(code, difficulty) {
  const text = normalize(code);
  const issues = [];

  if (!text) {
    issues.push("You have not written any pseudocode yet.");
    return issues;
  }

  if (
    text.includes("console.log") ||
    text.includes("printf") ||
    text.includes("system.out") ||
    text.includes("def ") ||
    text.includes("range(") ||
    text.includes("public static")
  ) {
    issues.push("This looks partly like a programming language, not plain IGCSE pseudocode.");
  }

  if (!/(input|read)/i.test(text)) {
    issues.push("Your answer does not clearly show where input is taken.");
  }

  if (!/(output|print|display)/i.test(text)) {
    issues.push("Your answer does not clearly show the final output.");
  }

  if (
    (text.includes("if") && !text.includes("endif")) ||
    (text.includes("while") && !text.includes("endwhile")) ||
    (text.includes("for") && !text.includes("next"))
  ) {
    issues.push("One or more control structures may not be closed properly.");
  }

  if (!text.includes("←") && !text.includes("<-") && !text.includes("=")) {
    issues.push("No assignment line is visible.");
  }

  if (difficulty === "Hard" && !/(declare)/i.test(text)) {
    issues.push("Hard mode expects clearer IGCSE style declarations where needed.");
  }

  return issues;
}

function markAnswer(question, answer, difficulty) {
  const breakdown = question.keyChecks.map((check) => {
    const hit = includesAny(answer, check.patterns, difficulty);
    return {
      ...check,
      awarded: hit ? check.marks : 0,
      hit,
    };
  });

  const rawScore = breakdown.reduce((sum, item) => sum + item.awarded, 0);
  const total = breakdown.reduce((sum, item) => sum + item.marks, 0);
  const percentage = total > 0 ? Math.round((rawScore / total) * 100) : 0;
  const issues = detectIssues(answer, difficulty);
  const strengths = breakdown.filter((item) => item.hit).map((item) => item.label);
  const missing = breakdown.filter((item) => !item.hit).map((item) => item.label);

  const nextStep = missing.length > 0
    ? `Work on this next: ${missing[0]}.`
    : "Nice work. Compare your answer to the model answer and tighten the exam style.";

  let gradeComment = "Keep practicing.";
  if (percentage >= 85) gradeComment = "Excellent. This is a strong exam style attempt.";
  else if (percentage >= 65) gradeComment = "Good job. Most key steps are present.";
  else if (percentage >= 40) gradeComment = "You have the main idea, but some important parts are missing.";
  else gradeComment = "This needs more structure. Use the hints and try again.";

  return {
    breakdown,
    rawScore,
    total,
    percentage,
    issues,
    strengths,
    missing,
    nextStep,
    gradeComment,
  };
}

function lineNumbers(text) {
  const count = Math.max(1, text.split("\n").length);
  return Array.from({ length: count }, (_, i) => i + 1).join("\n");
}

function buildRunPreview(question, answer) {
  const text = normalize(answer);
  if (!text) {
    return {
      status: "Waiting",
      output: ["Write your answer, then press Run or hit Enter."],
      notes: ["This preview estimates what your pseudocode would do."],
    };
  }

  if (question.id === 1) {
    return text.includes("total")
      ? {
          status: "Run complete",
          output: ["Input sample: 4, 7, 3, 6, 5", "Expected output: 25"],
          notes: ["Your answer looks like it is trying to total 5 numbers."],
        }
      : {
          status: "Partial run",
          output: ["Could not confirm the total result."],
          notes: ["Make sure you initialise Total, add each input, and output the final value."],
        };
  }

  if (question.id === 2) {
    return text.includes("mod") && text.includes("even") && text.includes("odd")
      ? {
          status: "Run complete",
          output: ["Input: 8 → Even", "Input: 5 → Odd"],
          notes: ["Your answer looks like an even or odd check."],
        }
      : {
          status: "Partial run",
          output: ["Could not confirm both Even and Odd outputs."],
          notes: ["Use MOD 2 and show both possible outputs."],
        };
  }

  if (question.id === 3) {
    return text.includes("largest")
      ? {
          status: "Run complete",
          output: ["Input sample: 8, 12, 4, 9, 7", "Expected output: 12"],
          notes: ["Your answer looks like it is finding the largest number."],
        }
      : {
          status: "Partial run",
          output: ["Could not confirm the largest value output."],
          notes: ["Store values, compare them, update Largest, then output it."],
        };
  }

  if (question.id === 4) {
    return (text.includes("repeat") || text.includes("while")) && text.includes("accepted")
      ? {
          status: "Run complete",
          output: ["Input: 15 → asks again", "Input: 7 → Accepted"],
          notes: ["Your answer looks like validation between 1 and 10 inclusive."],
        }
      : {
          status: "Partial run",
          output: ["Could not confirm the validation loop."],
          notes: ["Use repetition and stop only when the value is valid."],
        };
  }

  return text.includes("found") && text.includes("not found")
    ? {
        status: "Run complete",
        output: ["Search value: 7 → Found", "Search value: 11 → Not Found"],
        notes: ["Your answer looks like a search using a Found flag."],
      }
    : {
        status: "Partial run",
        output: ["Could not confirm both Found and Not Found outputs."],
        notes: ["Use a flag variable and output both possible results at the end."],
      };
}

function MenuButton({ active, icon: Icon, label, onClick }) {
  return (
    <Button
      variant={active ? "default" : "outline"}
      onClick={onClick}
      className={`justify-start rounded-2xl ${active ? "" : "border-slate-700 bg-slate-950 text-slate-200"}`}
    >
      <Icon className="mr-2 h-4 w-4" />
      {label}
    </Button>
  );
}

export default function App() {
  const [activeMenu, setActiveMenu] = useState("practice");
  const [questionIndex, setQuestionIndex] = useState(0);
  const [studentAnswer, setStudentAnswer] = useState("");
  const [showHints, setShowHints] = useState(false);
  const [showModelAnswer, setShowModelAnswer] = useState(false);
  const [difficulty, setDifficulty] = useState("Medium");
  const [lessonIndex, setLessonIndex] = useState(0);
  const [runPreview, setRunPreview] = useState(() => buildRunPreview(questionBank[0], ""));
  const textareaRef = useRef(null);

  const currentQuestion = questionBank[questionIndex];
  const currentLesson = lessons[lessonIndex];
  const result = useMemo(() => markAnswer(currentQuestion, studentAnswer, difficulty), [currentQuestion, studentAnswer, difficulty]);

  const filteredQuestions = useMemo(() => {
    if (difficulty === "Easy") return questionBank.filter((q) => q.level === "Beginner");
    if (difficulty === "Medium") return questionBank.filter((q) => q.level === "Beginner" || q.level === "Intermediate");
    return questionBank;
  }, [difficulty]);

  const currentFilteredIndex = Math.max(0, filteredQuestions.findIndex((q) => q.id === currentQuestion.id));

  const newQuestion = () => {
    const nextIndex = (currentFilteredIndex + 1) % filteredQuestions.length;
    const nextQuestion = filteredQuestions[nextIndex];
    const actualIndex = questionBank.findIndex((q) => q.id === nextQuestion.id);
    setQuestionIndex(actualIndex);
    setStudentAnswer("");
    setShowHints(false);
    setShowModelAnswer(false);
    setRunPreview(buildRunPreview(nextQuestion, ""));
  };

  const retryQuestion = () => {
    setStudentAnswer("");
    setShowHints(false);
    setShowModelAnswer(false);
    setRunPreview(buildRunPreview(currentQuestion, ""));
  };

  const runCode = () => {
    setRunPreview(buildRunPreview(currentQuestion, studentAnswer));
  };

  const handleEditorKeyDown = (e) => {
    if (e.key === "Enter" && !e.shiftKey) {
      e.preventDefault();
      runCode();
    }
  };

  const insertSnippet = (snippet) => {
    const textarea = textareaRef.current;
    if (!textarea) {
      setStudentAnswer((prev) => prev + snippet);
      return;
    }

    const start = textarea.selectionStart;
    const end = textarea.selectionEnd;
    const before = studentAnswer.slice(0, start);
    const after = studentAnswer.slice(end);
    const newValue = before + snippet + after;
    setStudentAnswer(newValue);

    requestAnimationFrame(() => {
      textarea.focus();
      const cursorPos = start + snippet.length;
      textarea.setSelectionRange(cursorPos, cursorPos);
    });
  };

  return (
    <div className="min-h-screen bg-slate-950 p-4 text-slate-100 md:p-8">
      <div className="mx-auto max-w-7xl space-y-6">
        <div className="flex flex-col gap-4 lg:flex-row lg:items-center lg:justify-between">
          <div>
            <div className="mb-2 flex items-center gap-2 text-sky-400">
              <Code2 className="h-5 w-5" />
              <span className="text-sm font-semibold uppercase tracking-[0.2em]">Pseudocode Pro Practice</span>
            </div>
            <h1 className="text-3xl font-bold tracking-tight text-white">IGCSE 0478 Pseudocode Trainer</h1>
            <p className="mt-2 max-w-3xl text-sm text-slate-400">
              Practice IGCSE 0478 pseudocode with instant marking, guided learning, run previews, and a clear marking scheme to help you improve step by step.
            </p>
          </div>
        </div>

        <div className="grid gap-6 xl:grid-cols-[240px_1fr]">
          <Card className="h-fit rounded-3xl border-slate-800 bg-slate-900 shadow-2xl">
            <CardHeader>
              <CardTitle className="text-lg text-white">Menu</CardTitle>
            </CardHeader>
            <CardContent className="space-y-3">
              <MenuButton active={activeMenu === "practice"} icon={Code2} label="Practice and marking" onClick={() => setActiveMenu("practice")} />
              <MenuButton active={activeMenu === "learn"} icon={GraduationCap} label="Learning" onClick={() => setActiveMenu("learn")} />
              <MenuButton active={activeMenu === "scheme"} icon={ClipboardList} label="Marking scheme" onClick={() => setActiveMenu("scheme")} />
            </CardContent>
          </Card>

          <div>
            {activeMenu === "practice" && (
              <div className="space-y-6">
                <Card className="rounded-3xl border-slate-800 bg-slate-900 shadow-2xl">
                  <CardHeader>
                    <CardTitle className="flex items-center gap-2 text-xl text-white">
                      <HelpCircle className="h-5 w-5 text-sky-400" /> Challenge
                    </CardTitle>
                  </CardHeader>
                  <CardContent className="space-y-5">
                    <p className="text-sm text-slate-400">Read the question, write your pseudocode answer below, run a preview, and then check your marks and feedback.</p>
                    <div className="flex flex-wrap gap-2">
                      <Badge variant="secondary">Topic: {currentQuestion.topic}</Badge>
                      <Badge variant="secondary">Question level: {currentQuestion.level}</Badge>
                      <Badge variant="secondary">Marking mode: {difficulty}</Badge>
                      <Badge variant="secondary">Marks: {currentQuestion.marks}</Badge>
                    </div>
                    <div className="flex flex-wrap gap-2">
                      <Button variant={difficulty === "Easy" ? "default" : "outline"} onClick={() => setDifficulty("Easy")}>Easy</Button>
                      <Button variant={difficulty === "Medium" ? "default" : "outline"} onClick={() => setDifficulty("Medium")}>Medium</Button>
                      <Button variant={difficulty === "Hard" ? "default" : "outline"} onClick={() => setDifficulty("Hard")}>Hard</Button>
                      <Button variant="outline" onClick={retryQuestion}>Clear</Button>
                      <Button onClick={newQuestion}><RefreshCw className="mr-2 h-4 w-4" />Next question</Button>
                    </div>
                    <div className="rounded-2xl border border-slate-800 bg-slate-950 p-4 whitespace-pre-wrap text-sm text-slate-200">
                      {currentQuestion.question}
                    </div>
                  </CardContent>
                </Card>

                <div className="grid gap-6 xl:grid-cols-[1.35fr_0.9fr]">
                  <div className="space-y-6">
                    <Card className="rounded-3xl border-slate-800 bg-slate-900 shadow-2xl">
                      <CardHeader>
                        <CardTitle className="flex items-center gap-2 text-xl text-white">
                          <BookOpen className="h-5 w-5 text-sky-400" /> Answer editor
                        </CardTitle>
                      </CardHeader>
                      <CardContent className="space-y-4">
                        <p className="text-sm text-slate-400">Type your answer in exam style pseudocode. Press Enter to run a quick preview, or use the Run button.</p>
                        <ScrollArea className="w-full whitespace-nowrap pb-2">
                          <div className="flex gap-2 pb-2">
                            {snippets.map((snippet) => (
                              <Button
                                key={snippet}
                                variant="outline"
                                className="rounded-full border-slate-700 bg-slate-950 text-xs text-slate-200"
                                onClick={() => insertSnippet(snippet)}
                              >
                                {snippet.replace(/\n/g, " ↵ ")}
                              </Button>
                            ))}
                          </div>
                        </ScrollArea>

                        <div className="overflow-hidden rounded-2xl border border-slate-800 bg-slate-950">
                          <div className="flex items-center justify-between border-b border-slate-800 px-4 py-2 text-xs text-slate-400">
                            <span>pseudocode.0478</span>
                            <span>{studentAnswer.split(/\s+/).filter(Boolean).length} words</span>
                          </div>
                          <div className="grid min-h-[420px] grid-cols-[64px_1fr] font-mono">
                            {/* Line numbers */}
                            <pre className="select-none border-r border-slate-800 bg-slate-900 px-3 py-4 text-right text-xs leading-6 text-slate-500">
                              {lineNumbers(studentAnswer)}
                            </pre>

                            {/* Code editor */}
                            <Textarea
                              ref={textareaRef}
                              value={studentAnswer}
                              onChange={(e) => setStudentAnswer(e.target.value)}
                              onKeyDown={handleEditorKeyDown}
                              spellCheck={false}
                              className="min-h-[420px] resize-none border-0 bg-slate-950 px-4 py-4 font-mono text-sm leading-6 text-slate-100 focus-visible:ring-0"
                              placeholder="1  DECLARE Number : INTEGER
2  DECLARE Total : INTEGER
3  Total ← 0
4  FOR Counter ← 1 TO 5
5      INPUT Number
6      Total ← Total + Number
7  NEXT Counter
8  OUTPUT Total

Write your pseudocode here..."
                            />
                          </div>
                        </div>

                        <div className="flex flex-wrap gap-2">
                          <Button onClick={runCode}><Play className="mr-2 h-4 w-4" />Run code</Button>
                          <Button variant="outline" onClick={() => setShowHints((v) => !v)}>
                            <Lightbulb className="mr-2 h-4 w-4" />{showHints ? "Hide hints" : "Show hints"}
                          </Button>
                          <Button variant="outline" onClick={() => setShowModelAnswer((v) => !v)}>
                            <Sparkles className="mr-2 h-4 w-4" />{showModelAnswer ? "Hide model answer" : "Show model answer"}
                          </Button>
                        </div>

                        {showHints && (
                          <div className="rounded-2xl border border-amber-900 bg-amber-950/40 p-4">
                            <div className="mb-2 text-sm font-semibold text-amber-300">Hints</div>
                            <div className="space-y-2 text-sm text-amber-100">
                              {currentQuestion.hints.map((hint, index) => (
                                <div key={index} className="flex gap-2"><ChevronRight className="mt-0.5 h-4 w-4 shrink-0" /> <span>{hint}</span></div>
                              ))}
                            </div>
                          </div>
                        )}

                        {showModelAnswer && (
                          <div className="rounded-2xl border border-emerald-900 bg-emerald-950/40 p-4">
                            <div className="mb-2 text-sm font-semibold text-emerald-300">Model answer</div>
                            <pre className="whitespace-pre-wrap font-mono text-sm leading-6 text-emerald-100">{currentQuestion.modelAnswer}</pre>
                          </div>
                        )}
                      </CardContent>
                    </Card>
                  </div>

                  <div className="space-y-6">
                    <Card className="rounded-3xl border-slate-800 bg-slate-900 shadow-2xl">
                      <CardHeader>
                        <CardTitle className="flex items-center gap-2 text-xl text-white">
                          <TerminalSquare className="h-5 w-5 text-sky-400" /> Run preview
                        </CardTitle>
                      </CardHeader>
                      <CardContent className="space-y-4">
                        <p className="text-sm text-slate-400">This area estimates what your pseudocode would do using sample inputs.
                        </p>
                          <div className="mb-3 flex items-center justify-between">
                            <span className="text-sm font-semibold text-slate-200">Status</span>
                            <Badge variant="secondary">{runPreview.status}</Badge>
                          </div>
                          <div className="rounded-xl border border-slate-800 bg-black p-4 font-mono text-sm text-emerald-300">((line, index) => (
                              <div key={index}>{line}</div>
                            ))}
                          </div>
                          <div className="mt-4 space-y-2">
                            {runPreview.notes.map((note, index) => (
                              <div key={index} className="text-sm text-slate-400">{note}</div>
                            ))}
                          </div>
                        </div>
                      </CardContent>
                    </Card>

                    <Card className="rounded-3xl border-slate-800 bg-slate-900 shadow-2xl">
                      <CardHeader>
                        <CardTitle className="flex items-center gap-2 text-xl text-white">
                          <Trophy className="h-5 w-5 text-sky-400" /> Live mark
                        </CardTitle>
                      </CardHeader>
                      <CardContent className="space-y-4">
                        <div className="flex items-center justify-between">
                          <span className="text-sm text-slate-400">Score</span>
                          <span className="text-3xl font-bold text-white">{result.rawScore}/{result.total}</span>
                        </div>
                        <Progress value={result.percentage} />
                        <div className="flex items-center justify-between text-sm text-slate-400">
                          <span>Percentage</span>
                          <span>{result.percentage}%</span>
                        </div>
                        <Badge variant="secondary" className="rounded-full px-3 py-1 text-xs">{result.gradeComment}</Badge>
                      </CardContent>
                    </Card>

                    <Card className="rounded-3xl border-slate-800 bg-slate-900 shadow-2xl">
                      <CardHeader>
                        <CardTitle className="text-xl text-white">Marking</CardTitle>
                      </CardHeader>
                      <CardContent className="space-y-4">
                        <div>
                          <div className="mb-2 text-sm font-semibold text-slate-200">What you did well</div>
                          <div className="space-y-2">
                            {result.strengths.length > 0 ? result.strengths.map((item, index) => (
                              <div key={index} className="flex gap-2 rounded-2xl border border-emerald-900 bg-emerald-950/40 p-3 text-sm text-emerald-100">
                                <CheckCircle2 className="mt-0.5 h-4 w-4 shrink-0" />
                                <span>{item}</span>
                              </div>
                            )) : <div className="rounded-2xl border border-slate-800 bg-slate-950 p-3 text-sm text-slate-400">No strong points detected yet. Start with the hints.</div>}
                          </div>
                        </div>

                        <div>
                          <div className="mb-2 text-sm font-semibold text-slate-200">Improve next</div>
                          <div className="rounded-2xl border border-amber-900 bg-amber-950/40 p-3 text-sm text-amber-100">{result.nextStep}</div>
                        </div>

                        {result.issues.length > 0 && (
                          <div>
                            <div className="mb-2 text-sm font-semibold text-slate-200">Possible issues</div>
                            <div className="space-y-2">
                              {result.issues.map((issue, index) => (
                                <div key={index} className="rounded-2xl border border-rose-900 bg-rose-950/40 p-3 text-sm text-rose-100">{issue}</div>
                              ))}
                            </div>
                          </div>
                        )}
                      </CardContent>
                    </Card>

                    <Card className="rounded-3xl border-slate-800 bg-slate-900 shadow-2xl">
                      <CardHeader>
                        <CardTitle className="text-xl text-white">Mark breakdown</CardTitle>
                      </CardHeader>
                      <CardContent>
                        <ScrollArea className="max-h-[420px]">
                          <div className="space-y-3">
                            {result.breakdown.map((item, index) => (
                              <div key={index} className="rounded-2xl border border-slate-800 bg-slate-950 p-4">
                                <div className="flex flex-col gap-3">
                                  <div className="flex items-center justify-between gap-3">
                                    <div className="text-sm font-semibold text-slate-100">{item.label}</div>
                                    <Badge className="rounded-full px-3 py-1" variant={item.hit ? "default" : "secondary"}>
                                      {item.awarded}/{item.marks}
                                    </Badge>
                                  </div>
                                  <div className="text-xs text-slate-500">Looks for: {item.patterns.join(" , ")}</div>
                                </div>
                              </div>
                            ))}
                          </div>
                        </ScrollArea>
                      </CardContent>
                    </Card>
                  </div>
                </div>
              </div>
            )}

            {activeMenu === "learn" && (
              <div className="grid gap-6 xl:grid-cols-[300px_1fr]">
                <Card className="h-fit rounded-3xl border-slate-800 bg-slate-900 shadow-2xl">
                  <CardHeader>
                    <CardTitle className="flex items-center gap-2 text-xl text-white"><BrainCircuit className="h-5 w-5 text-sky-400" /> Learning topics</CardTitle>
                  </CardHeader>
                  <CardContent className="space-y-3">
                    {lessons.map((lesson, index) => (
                      <button
                        key={lesson.id}
                        onClick={() => setLessonIndex(index)}
                        className={`w-full rounded-2xl border p-4 text-left transition ${lessonIndex === index ? "border-sky-500 bg-sky-500/10" : "border-slate-800 bg-slate-950 hover:border-slate-700"}`}
                      >
                        <div className="text-sm font-semibold text-slate-100">{lesson.title}</div>
                        <div className="mt-1 text-xs text-slate-400">{lesson.summary}</div>
                      </button>
                    ))}
                  </CardContent>
                </Card>

                <div className="space-y-6">
                  <Card className="rounded-3xl border-slate-800 bg-slate-900 shadow-2xl">
                    <CardHeader>
                      <CardTitle className="text-2xl text-white">{currentLesson.title}</CardTitle>
                    </CardHeader>
                    <CardContent className="space-y-4">
                      <div className="rounded-2xl border border-slate-800 bg-slate-950 p-4 text-sm text-slate-300">
                        {currentLesson.summary}
                      </div>
                      {currentLesson.blocks.map((block, index) => (
                        <div key={index} className="rounded-2xl border border-slate-800 bg-slate-950 p-4">
                          <div className="mb-2 text-sm font-semibold text-slate-100">{block.heading}</div>
                          <div className="text-sm leading-6 text-slate-300">{block.body}</div>
                        </div>
                      ))}
                    </CardContent>
                  </Card>

                  <Card className="rounded-3xl border-slate-800 bg-slate-900 shadow-2xl">
                    <CardHeader>
                      <CardTitle className="text-xl text-white">Question walkthrough</CardTitle>
                    </CardHeader>
                    <CardContent className="space-y-4">
                      <div className="text-sm text-slate-300">Use this to learn how a full exam style solution is built.</div>
                      <div className="rounded-2xl border border-slate-800 bg-slate-950 p-4 whitespace-pre-wrap text-sm text-slate-200">
                        {currentQuestion.question}
                      </div>
                      <div className="rounded-2xl border border-slate-800 bg-slate-950 p-4">
                        <div className="mb-2 text-sm font-semibold text-slate-100">Main idea</div>
                        <div className="text-sm text-slate-300">{currentQuestion.concept}</div>
                      </div>
                      <div className="rounded-2xl border border-slate-800 bg-slate-950 p-4">
                        <div className="mb-2 text-sm font-semibold text-slate-100">How to build the answer</div>
                        <div className="space-y-2 text-sm text-slate-300">
                          {currentQuestion.steps.map((step, index) => (
                            <div key={index} className="flex gap-2"><ChevronRight className="mt-0.5 h-4 w-4 shrink-0 text-sky-400" /><span>{step}</span></div>
                          ))}
                        </div>
                      </div>
                      <div className="rounded-2xl border border-emerald-900 bg-emerald-950/30 p-4">
                        <div className="mb-2 text-sm font-semibold text-emerald-300">Model answer</div>
                        <pre className="whitespace-pre-wrap font-mono text-sm leading-6 text-emerald-100">{currentQuestion.modelAnswer}</pre>
                      </div>
                    </CardContent>
                  </Card>
 