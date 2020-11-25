# Custom Syntax Highlighting for Sublime 3, ELITE Language
This repository contains a set of files used to define a custom syntax highlighting scheme for the Sublime 3 text editor
The specific language, ELITE, is proprietary for a specific tool & so obscure that you are unlikely to ever touch it

### However, I have discovered a few things working on this project:
  - Sublime Text 3, released in late 2017, radically changed the format for defining custom syntax highlighting schemes
  - Most online documentation for Sublime syntax highlighting caters to the depreciated format (.TMlanguage)
  - Those that cater to the newer format (.sublime-syntax) are sparse & (in my opinion) not explained very well
  
Therefore, although you are unlikely to use the specific language defined here, I have supplied these files such that you may use them as a template for defining your own custom language syntax.

## The 3 Necessary Files
In order to define a custom syntax, you will need three files:
  1. A YAML syntax file, named ANY_NAME.sublime-syntax
  2. A JSON color scheme file, named ANY_NAME.sublime-color-scheme
  3. A JSON settings file, named ANY_NAME.sublime-settings

For my repository, ANY_NAME has been substituted with my langage, 'elite'. All three of these files should be placed in the /Packages/User folder, found somewhere in your home directory. On my Linux machine, the exact location is home/USER/.config/sublime-text-3/Packages/User. Those running Windows or OSX will need to locate the User filepath on your machines. 

### The YAML Syntax File
The bulk of your code will be inside this file, essentially a collection of RegEx expressions used to scan your file and assign them to a style, called a 'scope', defined in the color scheme file. 
This file will always start off with:
```
%YAML 1.2
---
name: NAME_YOUR_SYNTAX
file_extensions: [a,b,s]
scope: FULL_FILEPATH_TO_YOUR_COLORSCHEME_FILE

contexts:
```
The 'name' value is what Sublime will list your syntax as in its internal menu. In the file extension array, list any file extensions that should be read using this syntax definition - comma separated, and dont include the '.' (i.e. [html,css,js]). The 'scope' value is the name of your color scheme file with a full filepath from the system root.

Underneath 'contexts', you will create names for your various syntax rules. Note the spacing, colons, and hyphens used in my file - like python, the program is sensitive to these small syntaxtual details. The only mandatory scope is the 'main', wherein you will simply '- include:' every subsequent context you will define. Within the custom contexts, 'match' will contain your RegEx exression & 'scope' will map this rule to the custom style defined in your color scheme. Push/Pop are used for more complex(multiline) regex rules; they merely add/remove a scope from the stack according to the rules defined within the subcontext. 

### The JSON Color Scheme File
In this file, we will define the simple CSS rules used to style each syntax rule. 

```
{
    "name": "CHOOSE_A_NAME",
    "globals":
    {
        "background": "#282923",  //background color
        "foreground": "#EEEEEE",  //recommend against changing this
        "selection": "#AAAAAAAF", //highlighting color
        "caret": "white"          //cursor color
    },
```

The 'name' at the top of this file doesn'y matter very much. The 'globals' are used to define styles such as the background and foreground colors for the whole file - you can change these to match the theme you are using, I have them configured to match the default dark theme. 

```
"rules":
    //create a JSON entry for each syntax type
    [
        {   //comments are green & italic
            "name": "Comment",
            "scope": "comment",
            "foreground": "#72CF2B",
            "font_style": "italic",
        },
```

Once you get to the 'rules' section, you are simply creating a rule/scope for each regex rule you want to define in the syntax file. Once again, the 'name' doesnt matter. The 'scope' is important - it much exactly match the scope you mapped to the context in the syntax file. The 'foreground' defines the text color - it accepts any valid CSS color format, including hex, RGB, etc. Finally, the font_style can be used to make the text bold or italic, or omit the field entirely for normal text. There are likely more style options you can incorporate here, but for 99% of syntax highlighting, this should be sufficient. 

### The JSON Settings File
The third and final file is a little baby, and only contains two features. First, it copies the file extensions we defined in the YAML file, ensuring Sublime will use these syntax rules whenever it opens a file of this type (note, now that we are working in JSON, the extensions are now contained in double quotes -). Second, it maps these files to your color scheme file, ensuring Sublime will use this color scheme to style files of this type:
```
{
	"extensions":["a","b","s"],
	//force files with the above extension to use our custom color scheme file
	"color_scheme": "FULL_FILEPATH_TO_YOUR_COLORSCHEME_FILE"
}

```

This should be everything you need to get started - happy styling!
