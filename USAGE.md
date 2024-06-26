# Usage

## Requirements/Dependencies

You need to have command-line tools `fzf`, `curl`, `perl`, `fold` and `tput` installed.

## Usage as a shell script

Install this script into your path, e.g. in `~/bin`. At the first run, the script will download the personas from the repository and store them in `/tmp.

You do not have to call it `find-persona`, you can rename it to whatever you want.

## Basic Usage

Download/clone the repo. Make Bash script file `scripts/find-persona` executable.  
Run `scripts/find-persona` without any arguments. A popup will appear with a list of personas. Keep on typing some keywords. The fuzzy-finder `fzf` will narrow down the selection list and show its preview window inside the terminal.

| preview-window of fuzzy-finder `fzf`|
|----------|
|  Inside Terminal, after typing `scripts/find-persona` this should pop up  |
| ![fzf in action](img/screenshot-terminal-find-persona.png)  |
|  Use the arrow keys to scroll, or type more keywords to narrow down the list shown in left half  |

In the screenshot above,

- the left half of the preview window shows the available personas.
- the right half shows a persona definition that matches best.
- the highlighted text contains the selected promptfor the keyword(s) just typed.

Select one row and press enter. The script will then print the persona definition as a command `export CI=...` to the terminal.  (`CI` is short for "`C`ustom `I`nstruction" and is the name of the environment variable that I use in my subsequent commands. You can change it to whatever you want.)

Note that the `find-persona` script does _not actually export_ `CI` as an environment variable. You can do that manually if you want to use it in your script. Use copy and paste to do so.

I configured the script this way because

- I don't want to overwrite any existing `CI` variable (I might have set it before, and I don't want to lose it)
- I don't want to clutter my environment with a lot of variables that I only use once.

## My Personal Usage

My main use case for using these personas is calling `find-persona` in conjunction with the `explore_perplexity_api.py`  script from my [perplexity-api-search](https://github.com/knbknb/perplexity-api-search) repo.

Sometimes I call `explore_perplexity_api.py` with the `--persona` option.  
The scripts then set the `CI` environment variable to the persona that I want to use. Here, "Education,"CS Bootcamp Instructor":

```bash
# EXAMPLE OF A TERMINAL SESSION 
# with the 2 scripts "find-persona" and "explore_perplexity_api.py"

# Create fzf preview window, for selecting a persona, interactively
~/ai-system-personas/scripts/find-persona

# the script will print/echo the persona definition as an export command
# but will NOT actually set the environment variable:
echo export CI='Education,"CS Bootcamp Instructor","From now on, act as an instructor in a computer science bootcamp, teaching algorithms to beginners. You will provide code examples using python programming language. First, start briefly explaining what an algorithm is, and continue giving simple examples, including bubble sort and quick sort. Later, wait for my prompt for additional questions. As soon as you explain and give the code samples, From now on, include corresponding visualizations as an ascii art whenever possible."';

# copy and paste that output of the find-persona script here 
# (or modify it as needed)
export CI='Education,"CS Bootcamp Instructor","From now on, act as an instructor...';

# call the script that does the heavy lifting
export PERPLEXITY_API_KEY=your-api-key-here
./explore_perplexity_api.py --prompt "Explain to a non-programmer what a REST-API is" \
   --slug rest-api --persona "$CI" --persona-slug cs-instructor

# output will be saved to a file named final-output/rest-api-<MODELNAME>.md
```

The `--slug` argument is just a label that I use to keep track of my interactions with the AI system and the prompts that I've used. It will become part of the output filename. Use whatever you want as a slug phrase, but for ease of use, I recommend using a label that is unique to the prompt you're using, and the `--slug` should not contain blanks or special characters. (Same for the `--persona-slug` argument.)

## More Prompts and Personas

Some good collections, presented with good GUI Design in mind, and with a lot of additional information:

- [Prompt Library](https://docs.anthropic.com/claude/prompt-library) by Anthropic AI for Claude LLM
- [Prompts from top-rated GPTs in the GPTs Store](https://github.com/ai-boost/awesome-prompts) - Curated by AI-Boost
