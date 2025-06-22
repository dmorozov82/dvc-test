# DVC walkthrough guide

Here's a step-by-step guide on how to add new data to your DVC project, commit the changes, and then checkout different versions:

**Assumptions:**

*   You have a DVC project initialized (using `dvc init`).
*   You have a Git repository initialized (using `git init`).
*   You have a remote storage configured (using `dvc remote add`).

**Step 1: Add New Data**

1.  **Place the Data in Your Project:** Put the new data file(s) or directory in your DVC project. For example, let's say you have a new data file called `new_data.csv` that you want to add to the `data` directory.

2.  **Add the Data to DVC:** Use the `dvc add` command to tell DVC to track the new data.

    ```bash
    dvc add data/new_data.csv
    ```

    This command will:

    *   Create a `.dvc` file (e.g., `data/new_data.csv.dvc`) that tracks the data file.
    *   Add the data file to DVC's cache (if it's not already there).
    *   Update your `.gitignore` file to ignore the actual data file (but track the `.dvc` file).

**Step 2: Commit Changes to Git**

1.  **Add the `.dvc` File to Git:** Use `git add` to stage the `.dvc` file for commit.

    ```bash
    git add data/new_data.csv.dvc
    ```

2.  **Commit the Changes:** Use `git commit` to commit the changes to your Git repository.

    ```bash
    git commit -m "Add new data file: new_data.csv"
    ```

3.  **Push to Remote (Optional):** If you have a remote Git repository, push the changes.

    ```bash
    git push origin main  # Or your branch name
    ```

**Step 3: Push Data to DVC Remote**

1.  **Push the Data to DVC Remote:** Use the `dvc push` command to upload the data to your configured DVC remote storage.

    ```bash
    dvc push
    ```

    This command will:

    *   Check if the data is already in the remote storage.
    *   If not, upload the data to the remote storage.

**Step 4: Modify the Data (Example)**

1.  **Modify the Data File:** Make some changes to the `data/new_data.csv` file. For example, add a new row or modify some existing values.

**Step 5: Add and Commit the Modified Data**

1.  **Add the Modified Data to DVC:** Use `dvc add` again to update the DVC tracking information.

    ```bash
    dvc add data/new_data.csv
    ```

    DVC will detect the changes and update the `.dvc` file accordingly.

2.  **Commit the Changes to Git:**

    ```bash
    git add data/new_data.csv.dvc
    git commit -m "Modify data file: new_data.csv"
    git push origin main
    ```

3.  **Push to DVC Remote:**

    ```bash
    dvc push
    ```

**Step 6: Checkout Different Versions**

1.  **View Git History:** Use `git log` to see the commit history and identify the commit hashes for the different versions of your data.

    ```bash
    git log
    ```

2.  **Checkout a Specific Git Commit:** Use `git checkout` to switch to the desired commit.

    ```bash
    git checkout <commit_hash>
    ```

    Replace `<commit_hash>` with the commit hash of the version you want to restore.

3.  **Run `dvc checkout`:** After checking out the Git commit, run `dvc checkout` to restore the data versions associated with that commit.

    ```bash
    dvc checkout
    ```

    DVC will restore the `data/new_data.csv` file to the version that was tracked in the specified commit.

**Example Scenario: Reverting to the Original Data**

Let's say you want to revert to the original version of `data/new_data.csv` that you added in Step 1.

1.  **Find the Commit Hash:** Use `git log` to find the commit hash where you initially added `new_data.csv`.

2.  **Checkout the Commit:**

    ```bash
    git checkout <original_commit_hash>
    ```

3.  **Run `dvc checkout`:**

    ```bash
    dvc checkout
    ```

    DVC will now restore the `data/new_data.csv` file to its original state.

**Key Points:**

*   **Git for Code and Metadata:** Use Git to track your code, `.dvc` files, `dvc.yaml`, and other configuration files.
*   **DVC for Data:** Use DVC to manage your large data files and track their versions.
*   **`dvc add`:** Use this command to tell DVC to track new data or update the tracking information for existing data.
*   **`dvc push`:** Use this command to upload your data to the DVC remote storage.
*   **`dvc checkout`:** Use this command to restore the data versions associated with a specific Git commit.
*   **Commit messages:** Use descriptive commit messages to make it easier to track the changes to your data and pipelines.

By following these steps, you can effectively manage your data and code using DVC and Git, and easily switch between different versions of your data.
