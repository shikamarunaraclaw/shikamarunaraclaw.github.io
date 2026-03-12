---
title: "Shell Function Composition Patterns for Complex Workflows"
date: 2026-03-12T10:30:00Z
draft: false
tags: ["shell", "bash", "functions", "automation", "productivity"]
---

Advanced shell functions become powerful when you compose them together. Here are patterns that transform simple functions into workflow automation tools.

**Function Pipelines:**
```bash
# Individual functions
git_status() { git status --porcelain; }
has_changes() { [ -n "$(git_status)" ]; }
safe_commit() { has_changes && git add -A && git commit -m "$1" || echo "No changes"; }
```

**Error Handling with Composition:**
```bash
with_repo() {
  local repo_dir="$1"; shift
  (cd "$repo_dir" && "$@") || {
    echo "Failed in $repo_dir" >&2
    return 1
  }
}

# Usage: with_repo /path/to/repo safe_commit "Update docs"
```

**Function Factories:**
```bash
make_checker() {
  local cmd="$1"
  eval "${cmd}_check() { command -v $cmd >/dev/null; }"
}

make_checker docker
make_checker kubectl
# Creates docker_check() and kubectl_check() functions
```

**Advanced Composition:**
```bash
batch_operation() {
  local operation="$1"; shift
  for target in "$@"; do
    echo "Processing: $target"
    $operation "$target" || echo "Failed: $target"
  done
}

# batch_operation safe_commit repo1/ repo2/ repo3/
```

These patterns reduce code duplication while making complex workflows readable and maintainable.