# 🎨 Penpot Tailwind-based Token & Design System

This repository contains a set of design tokens for Penpot, designed to replicate the naming conventions and values of the Tailwind CSS utility-first framework.

The system is built on a two-layer structure:

1. **Primitives:** A base set of raw, constant values from Tailwind CSS.

2. **Foundation:** A semantic layer of tokens that reference the primitives.

This structure allows all "Foundation" tokens to be globally modified using an affine transformation (linear scaling and offset). This makes it easy to quickly change the feel of your design for testing purposes by globally adjusting spacing, font sizes, or radii without manually editing hundreds of tokens & components.

## 📦 Token Set Structure

The system is organized into three main token sets.

### 1. `tailwind-primitives (don't use)`

- **Purpose:** This set acts as the single source of truth for all raw Tailwind values (colors, spacing, font sizes, etc.).

- **Usage:** Do !\*not! use these tokens directly in your designs. It exists only to be referenced by the Foundation set.

### 2. `Foundation`

- **Purpose:** This is the primary token set you will use in your Penpot designs. It contains tokens in tailwind naming convention (e.g., `spacing.04`, `text.base`, `radius.lg`). Note that the dashes `-` have been replaced with dots `.`, for a more organized/structured json file.

- **How it works:** Every token in this set is an alias. Instead of a static value, it's a formula that references a primitive and a transformation value.

- **Example:** The value for `Foundation.spacing.04` is not `16px` . It is:

```
{\_spacing.04} \* {affine-transform.spacing.scaling} + {affine-transform.spacing.offset}
```

### 3. `Theme config: Base`

- **Purpose:** This set provides the default transformation values for the Foundation set.

- **How it works:** It defines all the affine-transform tokens. In the "Base" theme, all `scaling` values are `1` and all `offset` values are `0` (meaning you are working with the standard, unchanged tailwind constants/primitives).

- **Example:**

  - `affine-transform.spacing.scaling: 1`

  - `affine-transform.spacing.offset: 0`

When the `Foundation` and `Theme config: Base` sets are combined in a theme, the formula for `Foundation.spacing.04` is resolved as: `16px \* 1 + 0`, resulting in the expected `16px`.

## 🚀 Usage Guide

### 1. Initial Setup

1. **Import Tokens:** In your Penpot file, go to the "Tokens" tab and import the `helionox_tailwind_token_system.json` file.

2. **Optional:** Verify that everything is in working order. Create an element and style it using `Foundation` tokens (i.e. add some margin to some text on a board). Then change the according value in `Theme config: Base`, i.e.: set `affine-transform.spacing.scaling` to `2` and check wether the spacing increased.

### 2. How to Create a Scaled Theme (e.g., "Compact")

This is the primary strength of this system. Let's create a "Compact" theme where all spacing and radii are 80% of the standard size.

1. **Duplicate the Base Config:**

- In the **"Tokens"** tab, click the three-dot menu on the `Theme config: Base` set and select **"Duplicate"**.

- Rename the new set to `Theme config: Compact`.

2. **Modify the New Config:**

- Inside your new `Theme config: Compact` token set, navigate to the tokens you want to change.

- Change `affine-transform.spacing.scaling` from 1 to 0.8.

- Change `affine-transform.border-radius.scaling` from 1 to 0.8.

3. **Create the New Theme:**

- Go to the **"Themes"** section (above to "Sets").

- Click the `Edit` button and `Add new theme` to create a new theme.

- Name the theme **"Compact"** and it to the "Group" **`Tailwind Config`**

- Check the set you just created: **`Compact`**

- **Uncheck** everything else

4. **Activate and Use:**

- Activate your new "Compact Theme".

- Now, any element using a **Foundation** token will be automatically transformed.

- `Foundation.spacing.04` (16px) will now resolve as `16px \* 0.8 + 0 = 12.8px`.

- `Foundation.radius.lg` (8px) will now resolve as `8px \* 0.8 + 0 = 6.4px`.

You can repeat this process to create any number of themes, like a "Large Type" theme (by increasing `affine-transform.font-size.scaling`) or a "Touch" theme (by increasing `affine-transform.spacing.scaling`).
