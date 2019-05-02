---
title: Blog Update March 14th
date: "2019-03-14"
---

##Gator Grader 2.0

To this point in the semester we have been updating the gatorgrader program for entry level students to be able to test and check their code before their final submission. We split these updates and improvement tasks into teams of 5. My team was tasked with the challenge of parsing over commit messages and ensuring that when the parser that is implemented to count the ammount of words in the code as well as count how many lines of code and comments there are in the submission was going to correctly count the lines in different subsets in markdown files.

These subsets include things such as lists, split paragraphs, as well as parsing over code blocks in markdown syntax. The first step we took was identifying what subsets the program incorrectly parsed or did not parse at all were and how travis reads markdown. We identified lists as one major one as the word count was the same when we had and did not have a list. As well as paragraph count would be two if there was a list in the same paragraph block instead of counting it as one whole paragraph. I worked on parsing over commit messages and ensuring the word count was present and people were not just putting random commit mesages (i.e. "asdf", "commit", "I hate my life", etc.)

Commit messages are very important when it comes to large software projects because they can help people who are reading the code for the first time better understand the organization of the program. This can also lead to a lot of time saved when it comes to looking back over code you may not have seen in a while and being able to see exactly what each bit of code that you added does.

##Pytest Commands

Another Issue I was assigned in gatorgrader was adding new pytest commands that would allow for students to be able to run specific tests that failed or passed. Pytest has all these functions so it was a matter of researching code that the students would be able to learn quickly and provide examples as well. Below is an example of using ```-rfs```. ```-r``` options accepts a number of characters after it, with a used above meaning “all except passes”. ```-f``` specifies failed and ```-s``` specifies skipped test. :




```
$ pytest -rfs
=========================== test session starts ============================
platform linux -- Python 3.x.y, pytest-4.x.y, py-1.x.y, pluggy-0.x.y
cachedir: $PYTHON_PREFIX/.pytest_cache
rootdir: $REGENDOC_TMPDIR, inifile:
collected 6 items

test_example.py .FEsxX                                               [100%]

================================== ERRORS ==================================
_______________________ ERROR at setup of test_error _______________________

    @pytest.fixture
    def error_fixture():
>       assert 0
E       assert 0

test_example.py:6: AssertionError
================================= FAILURES =================================
________________________________ test_fail _________________________________

    def test_fail():
>       assert 0
E       assert 0

test_example.py:14: AssertionError
========================= short test summary info ==========================
FAILED test_example.py::test_fail
SKIPPED [1] $REGENDOC_TMPDIR/test_example.py:23: skipping this test
 1 failed, 1 passed, 1 skipped, 1 xfailed, 1 xpassed, 1 error in 0.12 seconds
Using p lists the passing tests, whilst P adds an extra section “PASSES” with those tests that passed but had captured output:

$ pytest -rpP
=========================== test session starts ============================
platform linux -- Python 3.x.y, pytest-4.x.y, py-1.x.y, pluggy-0.x.y
cachedir: $PYTHON_PREFIX/.pytest_cache
rootdir: $REGENDOC_TMPDIR, inifile:
collected 6 items

test_example.py .FEsxX                                               [100%]

================================== ERRORS ==================================
_______________________ ERROR at setup of test_error _______________________

    @pytest.fixture
    def error_fixture():
>       assert 0
E       assert 0

test_example.py:6: AssertionError
================================= FAILURES =================================
________________________________ test_fail _________________________________

    def test_fail():
>       assert 0
E       assert 0

test_example.py:14: AssertionError
========================= short test summary info ==========================
PASSED test_example.py::test_ok
================================== PASSES ==================================
_________________________________ test_ok __________________________________
--------------------------- Captured stdout call ---------------------------
ok
 1 failed, 1 passed, 1 skipped, 1 xfailed, 1 xpassed, 1 error in 0.12 seconds
```
