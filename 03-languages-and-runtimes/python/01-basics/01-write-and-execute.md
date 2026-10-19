---
Created: 2026-10-19
Modified:
---

# Write and Execute Python Code

Python source files typically use the `.py` extension and contain instructions executed by a Python interpreter. Let's create and execute a Python file that you can use to follow along with the examples throughout this guide.

> [!TIP]
> As you work through the Python Basics guides, you can reuse `learning.py` to experiment with different concepts or create separate files as needed. You can also comment out code you don't want to execute. Save your changes and execute the script again to see the results.

1. Open **VS Code**
2. **Connect to WSL** by clicking the `<>` remote connection icon in the bottom-left corner, if you are not already connected to **WSL: Ubuntu**.
3. Select **Terminal > New Terminal**.
4. Check your current working directory using `pwd`. For this guide, we'll work inside your home directory, which typically has the path `/home/<USERNAME>`.
   - `pwd` (**Print Working Directory**) displays the absolute path of your current directory (folder), beginning at the root directory `/`.

```bash
pwd
```

If you are not in your home directory, navigate there using `cd ~`.
   - `cd` (**Change Directory**) navigates to another directory, changing your current working directory.
   - `~` (**tilde**) represents the current user's home directory.

```bash
cd ~
``` 

5. Create a directory to organize your Python source files and navigate into it. This step is optional, but helps keep your learning files together. After executing these commands, your current working directory will be `/home/<USERNAME>/python`.
   - `mkdir` (**Make Directory**) creates a new directory. 

```bash
mkdir python
cd python
```

6. Create a Python source file. You can name it whatever you prefer, but Python filenames should generally use lowercase letters and underscores (`_`) to separate words, such as `calculate_total.py`. This guide uses `learning.py`.
   - `touch` creates an empty file if it doesn't exist or updates the timestamps of an existing file without modifying its contents.

```bash
touch learning.py
```

7. Open the Python file in VS Code so we can write and execute our first Python statement.
   - `code` is the VS Code command-line launcher, which we configured in our installation guide under `Git`.

```bash
code learning.py
```

8. Add the following Python code to `learning.py`, then **Save** (**File > Save**) if **Auto Save** is not enabled.
   - The `print()` function displays the specified value in the terminal. In this example, it displays the text `Hello, Python!`.

```python
print("Hello, Python!")
```

9. Execute the Python file using `python3` or `python` followed by the filename. We configured `python` to invoke Python 3 in our Python installation guide, so either command can be used. **You only need to execute one of the following commands.**
   - Both `python` and `python3` invoke the Python interpreter in our configured environment, which processes and executes the instructions in the specified file.

```bash
python3 learning.py
```

```bash
python learning.py
```

Both commands produce the same output in the terminal. The interpreter executes the statements in `learning.py`, and `print()` displays the specified text.
```text
Hello, Python!
```

---

[🔼 Back to Python Basics](README.md)