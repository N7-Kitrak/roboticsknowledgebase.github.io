@@ -1,106 +1,127 @@
# Debugging and Compiling ROS 2 Packages in Visual Studio Code

This tutorial explains how to set up **Visual Studio Code** for building and debugging ROS 2 packages effectively.

---
# Jekyll 'Front Matter' goes here. Most are set by default, and should NOT be
# overwritten except in special circumstances. 
# You should set the date the article was last updated like this:
date: 2020-05-11 # YYYY-MM-DD
# This will be displayed at the bottom of the article
# You should set the article's title:
title: Title goes here
# The 'title' is automatically displayed at the top of the page
# and used in other parts of the site.
---
This template acts as a tutorial on writing articles for the Robotics Knowledgebase. In it we will cover article structure, basic syntax, and other useful hints. Every tutorial and article should start with a proper introduction.

This goes above the first subheading. The first 100 words are used as an excerpt on the Wiki's Index. No images, HTML, or special formating should be used in this section as it won't be displayed properly.
## 🧱 1. Preparing for Debugging

Imagine this scenario: You comment out a line that creates a publisher in your ROS 2 C++ node. The code compiles fine, but running the node gives a **segmentation fault**, with no helpful error message.

To debug such issues, we'll configure **GDB debugging** inside VS Code.

If you're writing a tutorial, use this section to specify what the reader will be able to accomplish and the tools you will be using. If you're writing an article, this section should be used to encapsulate the topic covered. Use Wikipedia for inspiration on how to write a proper introduction to a topic.
---

In both cases, tell them what you're going to say, use the sections below to say it, then summarize at the end (with suggestions for further study).
## 🧰 2. Install GDB Server (If Needed)

## First subheading
Use this section to cover important terms and information useful to completing the tutorial or understanding the topic addressed. Don't be afraid to include to other wiki entries that would be useful for what you intend to cover. Notice that there are two \#'s used for subheadings; that's the minimum. Each additional sublevel will have an added \#. It's strongly recommended that you create and work from an outline.
Ensure `gdbserver` is installed:

This section covers the basic syntax and some rules of thumb for writing.
```bash
sudo apt update
sudo apt install gdbserver
```

### Basic syntax
A line in between create a separate paragraph. *This is italicized.* **This is bold.** Here is [a link](/). If you want to display the URL, you can do it like this <http://ri.cmu.edu/>.
---

> This is a note. Use it to reinforce important points, especially potential show stoppers for your readers. It is also appropriate to use for long quotes from other texts.
## ⚙️ 3. Running a Node with GDB Server

Use this command to run a ROS 2 node with GDB server:

#### Bullet points and numbered lists
Here are some hints on writing (in no particular order):
- Focus on application knowledge.
  - Write tutorials to achieve a specific outcome.
  - Relay theory in an intuitive way (especially if you initially struggled).
    - It is likely that others are confused in the same way you were. They will benefit from your perspective.
  - You do not need to be an expert to produce useful content.
  - Document procedures as you learn them. You or others may refine them later.
- Use a professional tone.
  - Be non-partisan.
    - Characterize technology and practices in a way that assists the reader to make intelligent decisions.
    - When in doubt, use the SVOR (Strengths, Vulnerabilities, Opportunities, and Risks) framework.
  - Personal opinions have no place in the Wiki. Do not use "I." Only use "we" when referring to the contributors and editors of the Robotics Knowledgebase. You may "you" when giving instructions in tutorials.
- Use American English (for now).
  - We made add support for other languages in the future.
- The Robotics Knowledgebase is still evolving. We are using Jekyll and GitHub Pages in and a novel way and are always looking for contributors' input.
```bash
ros2 run --prefix 'gdbserver localhost:3000' <package_name> <executable_name>
```

Entries in the Wiki should follow this format:
1. Excerpt introducing the entry's contents.
  - Be sure to specify if it is a tutorial or an article.
  - Remember that the first 100 words get used else where. A well written excerpt ensures that your entry gets read.
2. The content of your entry.
3. Summary.
4. See Also Links (relevant articles in the Wiki).
5. Further Reading (relevant articles on other sites).
6. References.
Replace `<package_name>` and `<executable_name>` with your specific values.

#### Code snippets
There's also a lot of support for displaying code. You can do it inline like `this`. You should also use the inline code syntax for `filenames` and `ROS_node_names`.
---

Larger chunks of code should use this format:
## 🐞 4. Configure the Debugger in VS Code

Create or edit the file `.vscode/launch.json`:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "C++ Debugger",
            "request": "launch",
            "type": "cppdbg",
            "miDebuggerServerAddress": "localhost:3000",
            "cwd": "/",
            "program": "/home/$USER/Workspaces/ros2_cpp_ws/install/udemy_ros2_pkg/lib/udemy_ros2_pkg/service_client"
        }
    ]
}
```
def recover_msg(msg):

        // Good coders comment their code for others.
> 📝 Replace the `program` path with your actual executable path inside `install/`.

        pw = ProtocolWrapper()
Now press `F5` to launch the debugger and catch segmentation faults right where they happen.

        // Explanation.
---

## 🔄 5. Use `--symlink-install` for Faster Builds

By default, `colcon build` copies files from `build/` to `install/`. You can make builds faster by creating symbolic links:

        if rec_crc != calc_crc:
            return None
```bash
colcon build --symlink-install
```
This would be a good spot further explain you code snippet. Break it down for the user so they understand what is going on.

#### LaTex Math Support
Here is an example MathJax inline rendering $ \phi(x\|y) $ (note the additional escape for using \|), and here is a block rendering:
$$ \frac{1}{n^{2}} $$
To switch to this method, first clean your workspace:

#### Images and Video
Images and embedded video are supported.
```bash
rm -rf build/ install/ log/
colcon build --symlink-install
```

![Put a relevant caption here](assets/images/Hk47portrait-298x300.jpg)
This is especially helpful when you're making small changes to files like `package.xml`, launch files, or interface definitions.

{% include video id="8P9geWwi9e0" provider="youtube" %}
---

{% include video id="148982525" provider="vimeo" %}
## 🔧 6. Automate Build and Debug Tasks with `tasks.json`

Add this to `.vscode/tasks.json`:

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "source /opt/ros/humble/setup.bash && colcon build --symlink-install"
        },
        {
            "label": "debug",
            "type": "shell",
            "command": "echo -e '\n\nRun the node using the following prefix: \n  ros2 run --prefix 'gdbserver localhost:3000' <package_name> <executable_name> \n\nAnd modify the executable path in .vscode/launch.json file \n' && source /opt/ros/humble/setup.bash && colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=RelWithDebInfo"
        },
        {
            "label": "test",
            "type": "shell",
            "command": "colcon test && colcon test-result"
        }
    ]
}
```

Then in VS Code:
- Press `Ctrl+Shift+B` to build.
- Open the command palette (F1) → "Run Task" → choose `debug` or `test`.
- Use the printed instructions to launch your node under GDB.

The video id can be found at the end of the URL. In this case, the URLs were
`https://www.youtube.com/watch?v=8P9geWwi9e0`
& `https://vimeo.com/148982525`.
---

## Summary
Use this space to reinforce key points and to suggest next steps for your readers.
## 📌 Final Tips

## See Also:
- Links to relevant material within the Robotics Knowledgebase go here.
- Open your ROS workspace at the root level (`~/ros2_ws`) in VS Code.
- The ROS VS Code extension adds tools like:
  - Sourced terminals
  - Node runners
  - ROS topic graph visualizers

## Further Reading
- Links to articles of interest outside the Wiki (that are not references) go here.
---

## References
- Links to References go here.
- References should be in alphabetical order.
- References should follow IEEE format.
- If you are referencing experimental results, include it in your published report and link to it here.
Happy debugging! 🛠️🐢
