# Trouble shooting

Not everything will be plain sailing. What do you do when it doesn't go green?
You might have already hit issues in previous exercises. If so, you will have already learned some techniques to help you out now.

## Tasks
1. Trigger the `7 Run Link Check` pipeline
   - Check if it passes or fails. If it fails, why does it fail, is there a real problem? If so, what action should be taken? 
   - Don't fix anything right now.
  
2. Trigger the Exercise 7.1 pipeline
   - check if it passes or fails
   - what should we do next?
   - lets do that, how has that worked out?
   - clear down any resources you have deployed

3. Trigger the Exercise 7.2 pipeline
   - check if it passes or fails
   - what should we do next?
   - lets do that, how has that worked out?
   - clear down any resources you have deployed

## Notes
For exercise 1, the pipeline failed because there were multiple broken links in the website. It is a real problem as we ideally don't want broken links on a website. We discussed whether or not we want the pipeline to fail or pass if broken links exist. I initially thought it should pass but was later convinced that it should fail because if it doesn't fail, no one would be aware of the existing broken links.
