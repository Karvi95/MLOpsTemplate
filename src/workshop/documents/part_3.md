
# Part 3: Use GitHub for Version Control and Automation

## Pre-requisites
- Part 0, 1, and 2
## Summary
Your team wants to learn how to automate and orchestrate common tasks such as environment setup, training, testing using GitHub Actions. To accomplish this, the following steps will be performed:
- Setup a centralized version control to keep track of project code and manage different feature development tracks and releases
- Learn how to automate and orchestrate common tasks such as environment setup, training, testing by setting up a unit test workflow to run when code is updated in your branch

## Steps
1. Create your own development branch where you can make and track changes. This branch will be your development area to create and test new code or pipelines before committing or merging the code into a common branch, such as ```integration```.

    How: 
    - Navigate to the repo if not already there by running ```cd PATH_TO_REPO``` with the proper path to the cloned location.
    - Run following command to create a new branch named "yourname-dev"
        ```bash
        git checkout -b yourname-dev
        ```
    - This will set the working branch to ```yourname-dev```. To check, run the following command:
        ```bash
        git branch
        ```
    ```json
        ACTION: Create your own branch
    ```

2. Create an automated unit test task such that will be triggered by pushing the code to your development/feature branch. Let's use the ```Feature_Engineering``` module as the automated unit test to run to make sure the module performs correctly.

    How:
    - Locate the file named ```my_unit_test.yml``` in the ```.github/workflows``` folder
    - Make the following updates to the file:
        - Update the secret name on line 26 by replacing the ```MY_AZURE_CREDENTIALS``` to match the GitHub secret name for your Service Principal that was created during step 0. It most likely has a name similar to ```AZURE_CREDENTIALS_USERNAME```. Your line will look something like this:
        ```yaml
        creds: ${{ secrets.MY_AZURE_CREDENTIALS }}
        ```
        - Update line 30 by replacing GROUP, WORKSPACE, and LOCATION with the properties (resource group name, workspace name, and location) of your Azure Machine Learning Workspace created in step 0.
    ```json
        ACTION: Update the my_unit_test.yml file with your secret name on line 26, and your Azure resources on line 30.
    ```

3. Next, review the contents in the ```my_unit_test.yml``` file to understand the steps and how it is being triggered:

    How:
    - Review the trigger on lines 2-9 to see how this workflow is being run automatically
        - The ```workflow_dispatch``` on line 3 allows the workflow to be run manually which can be useful when testing
        - Lines 4-9 is what is going to automatically trigger the workflow. It is being triggered on a push to any branch that isn't ```main``` or ```integration```. The changes in the push are also filtered to only include changes made to the ```feature_engineering``` module. 
    - Review the job starting on line 11 that has been created already that does the following steps:
        - Check out repo
        - Login into Azure
        - Create AML job to run feature engineering module using the [custom action](../../../.github/actions/aml-job-create/action.yaml) and the existing [feature engineering job file](../core/data_engineering/feature_engineering.yml)

4. Make changes to feature_engineering job file to ensure job will run successful
    > Note: you may have done this in part 2, but still check the file to make sure.

    How:
    - Locate the file named ```feature_engineering.yml``` in the ```.github/src/workshop/data_engineering``` folder
    - Replace the computer cluster name on line 25 with your compute cluster name
    ```json
        ACTION: Replace Compute Cluster name on line 25 in feature_engineering.yml
    ```

5. Now that the necessary changes have been made, the changes can be pushed to your feature branch which will trigger the feature_engineering_unit_test workflow.

    How:
    - Run the following commands in sequence to stage changes, commit them, and then push them to your repo:

    1. ```bash 
        git status
        ```
    2. ```bash 
        git add .
        ```
    3. ```bash
        git commit -am "a short summary of changes- insert summary"
        ```
    4. ```bash
        git push origin yourname-dev
        ```
        > Note: ```Git status``` shows the files that have been modified. It is useful for seeing the latest status of the files, but isn't necessary to commit changes.

    - Check to see if the workflow was properly triggered by going to your github repo and selecting the actions tab
    ```json
        ACTION: Push changes to feature branch
    ```

## The CI CD Workflow is Shown Below:
![pipeline](images/part3cicd.png)

## Success criteria
- A feature or development branch was created to track your changes
- Trigger was created on the workflow file ```my_unit_test.yml``` to run on a push to your feature branch
- Understand the additional updates that were made both the ```my_unit_test.yml``` and ```feature_engineering.yml``` file for it to use your secrets and AML resources
- Workflow was successfully triggered by pushing changes to your feature branch

## Reference materials
- [GitHub Actions](https://github.com/features/actions)
- [GitHub Actions Workflow Triggers](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)

---

## [To Next Part 4](part_4.md)
