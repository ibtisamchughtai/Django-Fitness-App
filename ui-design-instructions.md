The Global System Prompt
    Role: Act as an Expert UX/UI Designer and Senior Frontend Engineer.
    Task: Establish a global, reusable UI/UX design system and layout architecture for a multi-page "Fitness Tracker" web application based on a strict visual reference. You will generate the layout, global styles, and a universal preloader that applies to all pages (e.g., Dashboard, Analytics, Workouts, Profile, and Authentication).
1. Global Layout Architecture (Strictly Enforced)Every page generated must inherit the exact structural layout hierarchy from the reference layout:
    Global Header (Full Width): * Left: Sticky brand area with a neon green dumbbell icon and the text "FITNESS TRACKER".
        Right: Context-aware navigation links/icons (e.g., Login/Register for guests; Dashboard, Workouts, Profile, Logout for authenticated users) using clean, minimalist iconography.
    Main Content Window (Centered Container): * All unique page content (whether a login card, a dashboard grid, or a data table) must be contained within a centered, elevated glassmorphic or solid card structure.
        Maintain consistent padding, rounded corners, and subtle borders across all page variations to preserve the structural DNA.
    Global Footer (Fixed/Sticky Bottom): * Centered text reading: "© 2024 Fitness Tracker. Built with Django MVT Architecture." in a muted gray color.
2. Universal Preloader System
    Behavior: Implement a full-screen overlay preloader that triggers on every initial page load or view transition.
    Design: A minimalist, high-fidelity CSS animation featuring a pulsing neon lime green dumbbell or a glowing circular progress indicator centered on a dark overlay background.
    Exit State: Once the DOM content is fully loaded, the preloader must execute a smooth 0.4s ease-out fade-out and scale transition, cleanly revealing the main centered layout.
3. Global Design System & Theme
Apply these exact visual styles consistently across all components, forms, tables, and charts:
    Element  Style Specification: Primary BackgroundDeep dark mode canvas (e.g., #16181D).Component BackgroundElevated dark charcoal for cards, blocks, and containers (e.g., #1F232B).Accent ColorHigh-visibility neon lime green (e.g., #CCFF00) used for primary call-to-actions, active states, loading indicators, and highlights.TypographyClean, modern sans-serif (e.g., Inter or Roboto). Primary headers in white, labels/subtext in muted gray.Form InputsDark background fields, subtle gray borders, uppercase muted labels, and contextual neon-accented icons aligned to the left inside the input.ButtonsPrimary: Solid neon green background with dark bold text.Secondary/Outline: Dark background with a thin gray border, transforming smoothly on hover.
4. Application Framework & Scope
    Target Output: Provide the implementation structure (HTML/CSS/JS or framework-specific components) showing how this layout wraps around different views.
    Ensure the layout is completely fluid and responsive, beautifully translating from desktop monitors to mobile screens while maintaining the exact visual language.