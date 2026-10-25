# Reproduction Report: Issue #71

**Environment:** macOS 26.2 (arm64), Python 3.14.2, pytest 9.1.1

**Setup:** Used the existing `.venv` (`make setup` on a fresh clone).

**Steps:**
```bash
.venv/bin/pytest tests/unit/test_readme_parser.py -k heading_hierarchy --runxfail -v
```

**Expected:** The test's markdown contains `#`, `##`, `###` headings, so `_extract_heading_hierarchy` returns a non-empty list.

**Actual:** The fixture is indented 8 spaces, so the parser finds zero headings and `assert len(headings) > 0` fails.

**Evidence:**
```
>       assert len(headings) > 0
E       assert 0 > 0
E        +  where 0 = len([])
tests/unit/test_readme_parser.py:156: AssertionError
```
Without `--runxfail`, the test reports XFAIL.

**Conclusion:** Reproduced. The parser is correct; the fix belongs in the test (dedent the fixture, remove the xfail marker).
