# Discord Components V2 – Full Guide

**Discord Components V2** is a transformative update to Discord’s messaging API that enables developers to build highly flexible, organized, and interactive messages using a new, fully modular component system. Each message is composed of components—texts, images, buttons, media, sections, containers, and more—which can be arranged in almost any configuration for rich presentation and interaction[2][3][7].

## Features Overview

- **Flexible layouts:** Arrange text, media, and interactivity with total control.
- **Components everywhere:** Every element—text, button, image, file, section—is a component.
- **Nesting and grouping:** Build hierarchical layouts with containers and sections.
- **Interactive elements:** Add buttons and select menus inline and contextually.
- **Markdown support:** Style text using Discord Markdown such as `**bold**` and `# Headings`[7][9].
- **Limits:** Up to 40 components and 4,000 text characters per message[3][7].

## Getting Started

### Enabling Components V2

- **Set the V2 flag:** Add the `IsComponentsV2` flag (e.g., `MessageFlags.IsComponentsV2`) when sending a message.
- **Use the `components` field:** You cannot use the old `content`, `embeds`, `poll`, or `stickers` fields—V2 disables them[3][7].

Example (discord.js):

```js
await interaction.reply({
  components: [/* V2 components go here */],
  flags: MessageFlags.IsComponentsV2,
});
```

## Component Types

| Component      | Description                                                                 |
| -------------- | --------------------------------------------------------------------------- |
| Container      | Top-level grouping for other components; allows nesting and hierarchy       |
| Section        | Groups 1-3 text elements and one accessory (button or thumbnail)            |
| Text Display   | Markdown-formatted text block                                               |
| Media Gallery  | Displays a grid or list of images/media                                     |
| Thumbnail      | Small image for emphasis, usually in sections                               |
| Button         | Inline, interactive button for actions                                      |
| Select Menu    | Interactive dropdown menu for choices                                       |
| Separator      | Renders a visual divider for organization                                   |
| File           | Attaches and references files explicitly                                    |

## Usage Examples

### Simple Markdown Text

```js
import { TextDisplayBuilder, MessageFlags } from "discord.js";

const textComponent = new TextDisplayBuilder()
  .setContent("**Welcome** to Discord Components V2!");

await channel.send({
  components: [textComponent],
  flags: MessageFlags.IsComponentsV2,
});
```
*All content formatting (bold, headers, code, etc.) should use Discord markdown syntax[7][9].*

### Section with a Button

```js
import { SectionBuilder, ButtonBuilder, MessageFlags } from "discord.js";

const section = new SectionBuilder()
  .addTextDisplayComponents(
    t => t.setContent("Click the button below to learn more!")
  )
  .setButtonAccessory(
    b => b.setCustomId("details_btn")
         .setLabel("Learn More")
         .setStyle(1)
  );

await channel.send({
  components: [section],
  flags: MessageFlags.IsComponentsV2,
});
```

### Advanced Layout Example

```js
import {
  ContainerBuilder,
  SectionBuilder,
  SeparatorBuilder,
  TextDisplayBuilder,
  MediaGalleryBuilder,
  ButtonBuilder,
  MessageFlags
} from "discord.js";

const container = new ContainerBuilder()
  .addComponents(
    new SectionBuilder()
      .addTextDisplayComponents(
        t => t.setContent("# Announcement"),
        t => t.setContent("Check out our newest features below!")
      )
      .setButtonAccessory(
        b => b.setCustomId("feature_btn")
             .setLabel("Features")
             .setStyle(1)
      ),
    new SeparatorBuilder(),
    new MediaGalleryBuilder().addImages([
      "https://cdn.example.com/img1.png",
      "https://cdn.example.com/img2.png"
    ]),
    new TextDisplayBuilder().setContent("End of announcement.")
  );

await interaction.reply({
  components: [container],
  flags: MessageFlags.IsComponentsV2,
});
```

## Best Practices & Limitations

- Never use the old `content`, `embeds`, or `attachments` fields—only build with components[3][7].
- Reference each file through a File/Media component—no automatic previewing[3].
- Nested components count towards the 40-component message limit[3][7].
- The entire message text (across all text components) must not exceed 4,000 characters[3][7].
- Assign integer IDs to components if you want to reference or update them later (or let Discord auto-assign IDs)[3].
- Interactive user or role mentions inside text displays will trigger notifications—use `allowedMentions` to control this[7].

## Markdown Formatting in Components

You can style your messages using Discord-flavored markdown:

- `**bold**`, `*italic*`, `__underline____`, `` `code` ``, `> blockquote`, `# Heading`
- Lists, links, and spoilers are also supported

For a complete markdown reference, see:
- [Markdown Guide for Discord][9]

## References & Resources

- [Official Discord Developer Docs: Component Reference][1]
- [Display Components (discord.js Guide)][7]
- [Components V2 Documentation/Examples][3]
- [Markdown Cheat Sheet][9]

[1]: https://discord.com/developers/docs/components/reference  
[2]: https://discord.com/developers/docs/change-log/2025-04-22-components-v2  
[3]: https://disky.me/docs/interactions/componentsv2/  
[7]: https://discordjs.guide/popular-topics/display-components  
[9]: https://gist.github.com/matthewzring/9f7bbfd102003963f9be7dbcf7d40e51

[1] https://www.markdownguide.org/extended-syntax/
[2] https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks
[3] https://www.codecademy.com/resources/docs/markdown/code-blocks
[4] https://www.markdownguide.org/basic-syntax/
[5] https://elischei.com/syntax-highlighting-in-markdown-code-blocks/
[6] https://www.youtube.com/watch?v=IG9_EM5cw3U
[7] https://retype.com/components/code-block/
[8] https://www.jetbrains.com/help/hub/markdown-syntax.html
[9] https://docs.readme.com/rdmd/docs/code-blocks
[10] https://help.obsidian.md/syntax
