# Dotfiles Project - Task List

**Generated**: October 23, 2025  
**Based on**: dotfiles-requirements.md  
**Status**: Active

---

## Task Priority & Importance Legend

- **Priority**: P0 (Critical) → P3 (Low)
- **Importance**: HIGH, MEDIUM, LOW

---

## P0 - Critical Issues (Must Fix First) ✅ ALL COMPLETED

### TASK-001: Fix Shell Strategy Contradiction ✅ COMPLETED
- **ID**: TASK-001
- **Title**: Align Shell Strategy Across All Components
- **Priority**: P0
- **Importance**: HIGH
- **Status**: ✅ COMPLETED
- **Description**: 
  - Current state: `detect_shell()` sets `SHELL="bash"` but profiles specify `shell: zsh`
  - Requirements state: bash-first with zsh compatibility
  - Impact: Users get inconsistent behavior, profiles don't work as expected
  - Files affected: `install.sh`, all profile YAML files
- **Actions**:
  1. ✅ Keep `detect_shell()` setting `SHELL="bash"` (per requirements)
  2. ✅ Update all profiles to specify `shell: bash` (except wsl-dev which already has bash)
  3. ✅ Update `machine.yaml.example` to show `shell: bash`
- **Validation**:
  - [x] All profiles have `shell: bash`
  - [x] `detect_shell()` returns bash
  - [x] Installer creates bash-first configuration
  - [x] Zsh compatibility still works

---

### TASK-002: Delete Orphaned Config Directory ✅ COMPLETED
- **ID**: TASK-002
- **Title**: Remove Legacy config/ Directory and Files
- **Priority**: P0
- **Importance**: HIGH
- **Status**: ✅ COMPLETED
- **Description**:
  - Current state: `dotfiles/config/shell/` exists with old `.bashrc` and `.zshrc` files
  - These are no longer used and cause confusion
  - May cause symlink issues if installer accidentally points to them
  - Files found: `config/shell/.bashrc`, `config/shell/.zshrc`
- **Actions**:
  1. ✅ Delete entire `dotfiles/config/` directory
  2. ✅ Verify no code references `config/` path
  3. ✅ Update any symlink detection logic if needed
- **Validation**:
  - [x] `dotfiles/config/` directory does not exist
  - [x] No broken symlinks pointing to old config
  - [x] Installer runs without errors
  - [x] No code references deleted paths

---

### TASK-003: Update Stale File Path Comments ✅ COMPLETED
- **ID**: TASK-003
- **Title**: Fix File Path Comments in Shell Scripts
- **Priority**: P0
- **Importance**: MEDIUM
- **Status**: ✅ COMPLETED
- **Description**:
  - Current state: Comments reference old paths like `config/shared/shell/exports.sh`
  - Actual location: `shell/exports.sh`
  - Causes developer confusion and documentation debt
  - Found in: `exports.sh`, `functions.sh`, `prompt.sh`
- **Actions**:
  1. ✅ Update file path comments in `shell/exports.sh`
  2. ✅ Update file path comments in `shell/functions.sh`
  3. ✅ Update file path comments in `shell/prompt.sh`
  4. ✅ Verify all other shell scripts have correct paths
- **Validation**:
  - [x] All file header comments show correct current paths
  - [x] No references to `config/shared/` or `config/macos/` paths
  - [x] Documentation matches actual file locations

---

## P1 - High Priority Issues

### TASK-004: Add Comprehensive Error Handling
- **ID**: TASK-004
- **Title**: Implement Error Handling in Installer
- **Priority**: P1
- **Importance**: HIGH
- **Description**:
  - Current state: Commands can fail silently (e.g., `dscl`, `chsh`)
  - Requirements: "Errors should never pass silently"
  - Impact: Installation failures may go unnoticed
- **Actions**:
  1. Add error checking for `dscl` command (macOS shell detection)
  2. Add error checking for `chsh` command (shell change)
  3. Add fallback mechanisms when commands fail
  4. Add validation for critical paths before use
  5. Ensure all critical operations have try-catch equivalents
- **Validation**:
  - [ ] All OS detection commands have error handling
  - [ ] Shell change failures are caught and reported
  - [ ] User gets clear error messages with resolution steps
  - [ ] Installation continues gracefully after non-critical failures

---

### TASK-005: Implement or Remove Package Installation
- **ID**: TASK-005
- **Title**: Complete Package Installation Feature
- **Priority**: P1
- **Importance**: MEDIUM
- **Description**:
  - Current state: Code prints packages but doesn't install them (placeholder)
  - Requirements: Optional package installation via homebrew/apt
  - Impact: Users expect packages to install but they don't
- **Actions**:
  1. Implement actual package installation for macOS (homebrew)
  2. Implement actual package installation for Linux (apt)
  3. Add error handling for package installation failures
  4. Add progress indicators during installation
  5. OR: Remove the feature entirely if not needed
- **Validation**:
  - [ ] Packages actually install when user chooses 'y'
  - [ ] Failed packages are logged but don't stop installation
  - [ ] User sees clear progress during package installation
  - [ ] OR: Feature is completely removed if not implementing

---

### TASK-006: Verify Bash Deprecation Warning Suppression
- **ID**: TASK-006
- **Title**: Ensure macOS Bash Warning is Suppressed
- **Priority**: P1
- **Importance**: HIGH
- **Description**:
  - Requirements: Must suppress "The default interactive shell is now zsh" warning
  - Current: `BASH_SILENCE_DEPRECATION_WARNING=1` set in both `.bashrc` and `.zshrc`
  - Need to verify this works correctly in all scenarios
- **Actions**:
  1. Test that warning doesn't appear in new bash terminals
  2. Verify environment variable is set correctly
  3. Remove duplicate from `.zshrc` (only needed in `.bashrc`)
  4. Test with bash login shells vs non-login shells
- **Validation**:
  - [ ] No macOS zsh recommendation warning in new terminals
  - [ ] Warning suppressed in both login and non-login bash shells
  - [ ] Environment variable only set once (in `.bashrc`)
  - [ ] Works after fresh installation

---

## P2 - Medium Priority Improvements

### TASK-007: Test and Verify Idempotency
- **ID**: TASK-007
- **Title**: Ensure Installer is Truly Idempotent
- **Priority**: P2
- **Importance**: HIGH
- **Description**:
  - Requirements: "Safe to run multiple times"
  - Need to verify no duplicate configurations or broken symlinks
  - Test running installer 2-3 times in a row
- **Actions**:
  1. Run installer fresh on test system
  2. Run installer again without changes
  3. Verify backups are created each time
  4. Verify no duplicate entries in generated files
  5. Check for orphaned symlinks
  6. Verify shell doesn't change unnecessarily
- **Validation**:
  - [ ] Running installer twice creates separate backups
  - [ ] No duplicate lines in `.bashrc`, `.bash_profile`, `.zshrc`
  - [ ] Symlinks are updated, not duplicated
  - [ ] Shell change only happens if needed
  - [ ] All features work correctly after second run

---

### TASK-008: Optimize Shell Startup Performance
- **ID**: TASK-008
- **Title**: Ensure Shell Startup Time < 500ms
- **Priority**: P2
- **Importance**: MEDIUM
- **Description**:
  - Requirements: Shell startup time < 500ms
  - Need to measure and optimize if necessary
  - Check for slow operations in init scripts
- **Actions**:
  1. Add timing measurements to shell initialization
  2. Identify slow operations (completion loading, etc.)
  3. Optimize or lazy-load heavy operations
  4. Test startup time in bash and zsh
- **Validation**:
  - [ ] Bash startup time measured < 500ms
  - [ ] Zsh startup time measured < 500ms
  - [ ] No noticeable lag when opening new terminals
  - [ ] Completion and features still work

---

### TASK-009: Add Installation Time Tracking
- **ID**: TASK-009
- **Title**: Verify Installation Completes in < 30 Seconds
- **Priority**: P2
- **Importance**: LOW
- **Description**:
  - Requirements: Installation time < 30 seconds (without packages)
  - Need to measure actual installation time
  - Add progress indicators for user feedback
- **Actions**:
  1. Add timestamp at start of installation
  2. Add timestamp at end of installation
  3. Report total time to user
  4. Add progress indicators for long operations
- **Validation**:
  - [ ] Installation completes in < 30 seconds
  - [ ] User sees clear progress during installation
  - [ ] Time is reported in summary
  - [ ] No unnecessary delays

---

### TASK-010: Improve Installer Output Messages
- **ID**: TASK-010
- **Title**: Enhance User Experience with Better Messages
- **Priority**: P2
- **Importance**: MEDIUM
- **Description**:
  - Requirements: Clear colored output, helpful error messages, progress indicators
  - Current messages are good but could be more consistent
  - Add more context to error messages
- **Actions**:
  1. Review all echo statements for clarity
  2. Add more context to error messages
  3. Include resolution steps in error messages
  4. Ensure consistent emoji/icon usage
  5. Add progress percentages or steps
- **Validation**:
  - [ ] All messages are clear and actionable
  - [ ] Errors include resolution steps
  - [ ] User knows what step installer is on
  - [ ] Consistent formatting throughout

---

## P3 - Low Priority Enhancements

### TASK-011: Add Dry-Run Mode
- **ID**: TASK-011
- **Title**: Implement --dry-run Flag for Preview
- **Priority**: P3
- **Importance**: LOW
- **Description**:
  - Allow users to preview changes without making them
  - Show what would be backed up, what would be created
  - Useful for testing and documentation
- **Actions**:
  1. Add `--dry-run` flag to argument parsing
  2. Implement preview mode that shows actions without executing
  3. Show what files would be created/modified
  4. Show what would be backed up
- **Validation**:
  - [ ] `./install.sh --dry-run` shows preview
  - [ ] No files are actually modified in dry-run mode
  - [ ] Output is clear and matches actual run
  - [ ] Documented in help message

---

### TASK-012: Add Automated Tests
- **ID**: TASK-012
- **Title**: Create Test Suite for Installer
- **Priority**: P3
- **Importance**: MEDIUM
- **Description**:
  - No automated tests currently exist
  - Add tests for critical functionality
  - Use bats (Bash Automated Testing System) or similar
- **Actions**:
  1. Set up test framework (bats)
  2. Add tests for OS detection
  3. Add tests for shell detection
  4. Add tests for file generation
  5. Add tests for idempotency
  6. Add tests for backup creation
- **Validation**:
  - [ ] Test suite exists in `tests/` directory
  - [ ] Tests cover critical functionality
  - [ ] Tests pass on macOS, Linux, WSL
  - [ ] CI/CD integration possible

---

### TASK-013: Create Uninstall Script
- **ID**: TASK-013
- **Title**: Add --uninstall Option to Remove Dotfiles
- **Priority**: P3
- **Importance**: LOW
- **Description**:
  - Provide way to cleanly remove dotfiles
  - Restore from most recent backup
  - Remove symlinks and generated files
- **Actions**:
  1. Add `--uninstall` flag
  2. Find most recent backup directory
  3. Restore files from backup
  4. Remove symlinks
  5. Remove generated shell RC files
  6. Optionally change shell back
- **Validation**:
  - [ ] `./install.sh --uninstall` removes dotfiles
  - [ ] Original files are restored from backup
  - [ ] No orphaned symlinks remain
  - [ ] User can re-install afterwards

---

### TASK-014: Extract Installer into Modules
- **ID**: TASK-014
- **Title**: Refactor install.sh for Maintainability
- **Priority**: P3
- **Importance**: MEDIUM
- **Description**:
  - Current `install.sh` is 527 lines - too large
  - Extract logical components into separate libraries
  - Improve code organization and testability
- **Actions**:
  1. Create `lib/` directory
  2. Extract OS detection to `lib/os_detect.sh`
  3. Extract configuration loading to `lib/config.sh`
  4. Extract package management to `lib/packages.sh`
  5. Extract shell setup to `lib/shell_setup.sh`
  6. Main `install.sh` becomes orchestrator (~100 lines)
- **Validation**:
  - [ ] `install.sh` is < 150 lines
  - [ ] Code is organized into logical modules
  - [ ] Each module has single responsibility
  - [ ] Installation still works exactly the same
  - [ ] Code is more testable

---

### TASK-015: Document Common Issues in Troubleshooting Guide
- **ID**: TASK-015
- **Title**: Create Simple Troubleshooting Guide
- **Priority**: P3
- **Importance**: LOW
- **Description**:
  - Users may encounter common issues
  - Provide quick reference for solutions
  - Keep it minimal per user preference
- **Actions**:
  1. Create `TROUBLESHOOTING.md` (minimal)
  2. Document: "Aliases not loading" → solution
  3. Document: "Shell didn't change" → solution
  4. Document: "Permission denied" → solution
  5. Document: "Broken symlinks" → solution
- **Validation**:
  - [ ] Document exists and is concise
  - [ ] Covers 5-10 most common issues
  - [ ] Solutions are actionable
  - [ ] No duplication with requirements doc

---

## Task Summary by Priority

### P0 Critical (Must Do): ✅ ALL COMPLETED
- TASK-001: Fix Shell Strategy Contradiction ✅ COMPLETED
- TASK-002: Delete Orphaned Config Directory ✅ COMPLETED
- TASK-003: Update Stale File Path Comments ✅ COMPLETED

### P1 High Priority:
- TASK-004: Add Comprehensive Error Handling
- TASK-005: Implement or Remove Package Installation
- TASK-006: Verify Bash Deprecation Warning Suppression

### P2 Medium Priority:
- TASK-007: Test and Verify Idempotency
- TASK-008: Optimize Shell Startup Performance
- TASK-009: Add Installation Time Tracking
- TASK-010: Improve Installer Output Messages

### P3 Low Priority:
- TASK-011: Add Dry-Run Mode
- TASK-012: Add Automated Tests
- TASK-013: Create Uninstall Script
- TASK-014: Extract Installer into Modules
- TASK-015: Document Common Issues

---

## Recommended Execution Order

### ✅ COMPLETED (P0 Critical):
1. ✅ **TASK-002** → Delete orphaned files (quick, prevents confusion)
2. ✅ **TASK-001** → Fix shell strategy (resolves main contradiction)
3. ✅ **TASK-003** → Update comments (quick, improves clarity)

### 🔄 NEXT (P1 High Priority):
4. **TASK-006** → Verify bash warning suppression (test current implementation)
5. **TASK-004** → Add error handling (improves reliability)
6. **TASK-007** → Test idempotency (validate core requirement)
7. **TASK-005** → Implement/remove package installation (decide and execute)

### 📋 REMAINING (P2-P3):
8. **TASK-010** → Improve messages (better UX)
9. **TASK-008** → Optimize performance (if needed after testing)
10. **TASK-009** → Add timing (measurement for validation)
11. **TASK-011+** → Enhancement tasks as time permits

---

## Notes

- **Bash-first principle**: All tasks must maintain bash-first with zsh compatibility
- **Zen of Python**: Keep solutions simple, explicit, and readable
- **No over-engineering**: Only implement what's needed
- **Test each task**: Validate on macOS before moving to next task
- **User preference**: Minimal documentation, focus on working code

---

**Last Updated**: October 23, 2025  
**Status**: P0 Critical tasks completed ✅ | Ready for P1 High Priority tasks

