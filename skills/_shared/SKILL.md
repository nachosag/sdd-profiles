---
name: _shared
description: "Shared reference recipes for sdd-profiles skills. Not invokable."
license: MIT
metadata:
  author: "nachosag"
  version: "1.0"
disable-model-invocation: true
user-invocable: false
---

## Purpose

This directory contains the reference recipes shared by the sdd-profiles skills:
- `fetch-subscription.md` — procedure for fetching the current data of a subscription.
- `generate-profile.md` — procedure for generating a model assignment profile.

## Not Invokable

This package is support-only. It is not invoked directly; the `sync-sdd-profiles`, `update-sdd-profiles`, and `create-sdd-profile` skills reference these recipes.
