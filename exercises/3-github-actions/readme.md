# Build and test an app

## Task Instructions
There is a basic node app provided in the `app` folder. It gets all its dependencies publically from nodejs.

We need a pipeline to 
- checkout the code, 
- install the dependencies, 
- check for outdated packages
- prettify the code and 
- run the tests.

We would like the pipeline to run on each push to `main`

- Create the configuration for the pipeline in the `exercise_3_app_build.yaml` file including name, trigger, and job that runs on ubuntu-latest.
- Add steps to the job which check out the code, support the node environment and run the necessary commands.
- Move the config file into `.github/workflows` folder and commit & push your code.
- Use the github web interface to find the pipeline and trigger it, then check it has run correctly.

## Hints
- Check which folder the commands need to run in
- Check the version of node for the project in the `.nvmrc` file
- Be aware that for safety, you will want to use pnpm to install and manage packages and scripts
- Check the available runnable pnpm `scripts` in the `package.json` file

## If you're stuck
- Review the helpful resources in the documentation folder
- Look at the `example.yaml` in the workflows folder, this will not solve it all but might get you started

## Stretch tasks
How would you temporarily suspend the pipeline so it didn't run on a push?
For discussion: What else is missing from this pipeline? (no need to add it right now)

## Notes

I used a workflow template from GitHub which helped me with this exercise. However, the template contained a step to use `actions/setup-node@v4` with a specified Node version. Since we were using pnpm in our workflow, these steps seemed to clash and this caused my workflow to take significantly longer in the `install dependancies` step which resulted in this step taking approximately 90 seconds (compared to about 10 seconds for others).

The discussion on whether the outdated_check step should cause the pipeline to pass or fail was interesting and I learnt that we could allow our pipeline to pass if a non-critical step fails. Since we weren't interested in updating our packages if we had any outdated packages, we allowed our pipeline to pass by using `pnpm exec outdated_check || true`. Another way to allow the pipeline to pass even if the outdated_check failed was by setting the `continue-on-error` setting to `true`.
