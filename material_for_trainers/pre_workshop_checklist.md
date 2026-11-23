# Pre-workshop checklist

## Branch
- Make a branch of this repository using the date format `yyyy-mm-dd`
- Do any modifications to files of this repo only on that branch
- Clear the `material_for_participants/command.log` file
- Make an [edu.nl](https://edu.nl/) link to point to `material_for_participants` folder of the new branch


## `Links` document
- Update the `links.md` file with any links participants will need.
- Currently we are using good-old pen and paper for roll call. So the current file does not need updating per workshop run.
- [OPTIONAL] If you have a feedback survey for this workshop, paste the link in `links.md`

## Slides
- Update the [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/branching%20and%20merging%20with%20git.pptx?d=w33d15b9f24e94794aaa7624d5b908dd3&csf=1&web=1&e=rmUzca)
    - Names of trainers/helpers
    - Modify schedule (if needed)
    - Add new `edu.nl` link

## Lesson prep

- Pick an icebreaker from the [resources document](https://tud365.sharepoint.com/:w:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/resources.docx?d=waea671d7fc6a46d5b5c068fc19f41940&csf=1&web=1&e=f2QYgy)
- Practice teaching the material on your own (see [lesson plan spreadsheet](https://tud365.sharepoint.com/:x:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/lesson_plan.xlsx?d=we808cfe275964b25a61e1fa97fc31664&csf=1&web=1&e=LYLTGt) for links to content)

- Prepare a separate device to have during the lesson.
    - Use it to visualize `materials_for_trainers/lesson_plan.md` file (in this repo).

## Set up autopush for live coding

- Clone the https://github.com/tu-delft-library/intermediate_version_control_with_git repository to a `<local-repo-directory>`
- History will be saved to `material_for_participants/command.log`
- Bash doesn't automatically save the history. Set it up by adding this command to `~/.bashrc`
    ```bash
    PROMPT_COMMAND='history -a'
    ```
- On workshop day, you need to **START AUTOPUSH**. Instructions can be found in `materials_for_trainers/lesson_plan.md`
- Original instructions on setting up autopush can be found[here](https://github.com/4TUResearchData-Carpentries/workshop_notes)

## Clean up your system
- Delete the `weather-notes` repo from your github
- Delete `weather-notes` from your `Desktop`
- If you are using a mac, make the terminal not transparent: 
    - Open the terminal
    - Open settings
    - Go to background color
    - Adjust opacity to 100%
    
- Set your desktop to a color background instead of an image that can be distracting (e.g. a beach or a mountain)
