# UHFPS Objectives Skill

## Purpose

Use this skill when adding objective progression, sub-objectives, objective
triggers, or puzzle completion flow.

## Core Idea

Use UHFPS Objectives assets and Objective Manager flow. Do not create a separate
quest or objective system.

## UHFPS Concepts

- Objectives live in an Objectives asset.
- Objectives can contain sub-objectives.
- Objective triggers can activate through trigger, interact, or event.
- Count selectors can show progress.

## Rules

- Do not hardcode objective UI separately from UHFPS objectives.
- Trigger objectives through UHFPS objective flow.
- Keep objective text clear but not overly explanatory for horror pacing.
- Review save/load for objective completion state.

## Setup Checklist

- Objectives asset created or updated
- sub-objectives configured
- Objective Manager has the correct asset
- Objective Trigger component attached when needed
- trigger type selected
- event call documented if using event activation
