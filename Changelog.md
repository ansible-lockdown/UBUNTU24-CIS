# Ubuntu24CIS

## Based on CIS v1.0.0 - September 2026

### Fixed

- 2.2.1 and 2.3.3.3 carried no patch or audit tag, so `--tags "level1-server,patch"` skipped them and the remediation never ran
- 4.2.5, 6.1.1.1, 6.1.1.3, 6.1.2.1.2, 6.1.3.8 and the journald conf.d helper change system state but were tagged audit only - patch added beside audit, so existing audit runs are unaffected
- templates/usr/share/pam-configs/pam_unix.j2 renamed to unix.j2: 5.3.2.1 resolves its src to `unix.j2`, so the task failed with "template not found" whenever `ubtu24cis_pam_create_pamunix_file` was set
- unix.j2 Password stanza now ends `{% endif +%}`: trim_blocks removed the newline and joined the line onto `Password-Initial:`, so pam-auth-update wrote a junk option into /etc/pam.d/common-password and the configured hash algorithm was never applied
- 7.1.13 used `find ... -perm \( -02000 or -04000 \)`; find takes `\(` as the mode argument and `or` is not a find operator, so the SUID/SGID review reported clean on every host
- 3.2.1-3.2.4 modprobe tasks looped two lines against ONE shared regexp, so the blacklist item overwrote the install item and only `blacklist <mod>` survived - each item now carries its own regexp, matching the section 3.1 pattern. The doubled-backslash `\\s` in those regexps (a literal backslash, which never matched) was masking it: correcting the regexp alone makes the overwrite start working and BREAKS the control
- 3.2.1 wrote `blacklist cramfs` into blacklist.conf instead of `blacklist dccp` - both the regexp and the line were copy-pasted from the cramfs control
- pre_remediation_audit.yml: the git-core install had no `lock_timeout`, aborting a converge on a freshly booted host
- .github/workflows: actions/checkout pinned to v7.0.0
- vars/is_container.yml disabled `ubtu24cis_rule_1_3_1` to `_1_3_4`, IDs this role does not define - the AppArmor controls are 1.3.1.1 to 1.3.1.4, so containers were never actually skipping them; also dropped `ubtu24cis_rule_6_2_4_11`, which does not exist (6.2.4 runs to _10)
- 7.2.9/10: the shared home-directory discovery was gated `when: [rule_7_2_9, rule_7_2_10]`, which ANDs - disabling either control skipped the discovery while the other still looped its register, failing the play. Now ORed, matching the 5.4.2.7/8 sibling
- 1.1.2.3.1: the /home partition warning was gated on `discovered_dev_shm_mount`, a copy-paste from 1.1.2.2.1, so it never reflected /home's actual mount state. The six sibling partition controls were already correct
- 1.1.2.3.1, 1.1.2.4.1, 1.1.2.5.1, 1.1.2.6.1 and 1.1.2.7.1 are Level 2 in the benchmark but were tagged level1, so a `--tags level2-server` run skipped them and a Level 1 run wrongly included them
- templates/audit/98_auditd_exception.rules.j2 referenced the unprefixed `allow_auditd_uid_user_exclusions`; a bare undefined name in a Jinja `{% if %}` raises AnsibleUndefinedVariable, so enabling the exclusions feature failed the run
- prelim.yml gained the snap and squashfs discovery tasks: `templates/lockdown_audit.yml.j2` reads `prelim_snap_pkg_mgr` and `prelim_squashfs_builtin`, which this role never registered, so `ubtu24cis_squashfs_skip` always rendered false and the audit never skipped the control on a snap-based system
- 5.1.4: the goss vars emitted `AllowUsers`/`AllowGroups`/`DenyUsers`/`DenyGroups` even when the access list is empty, so the audit asserted the directive was present and failed on every host at default settings - the keyword now only renders with a non-empty list

## Based on CIS v1.0.0 - August 2026

### Fixed

- tasks/section_3/cis_3.1.x.yml: 3.1.2 used `command` with a shell glob, so the wireless discovery never matched and the control never applied in any configuration
- tasks/section_6/cis_6.3.x.yml: 6.3.2 masked the dailyaidecheck units with a hardcoded `state`, failing the play when aide was absent - now uses the package-presence ternary
- CONTRIBUTING.md replaced with the canonical version, README Contributing section added
- README.md: X badge still used the twitter.com shields endpoint
- tasks/section_4/cis_4.4.3.x.yml: `level1-workstation` tag carried a stray non-ASCII character, so the task was skipped in tagged workstation runs
- 16 controls carried level tags that disagreed with the benchmark Profile Applicability: 6.2.4.1-6.2.4.10 and 6.3.3 are Level 2, 1.7.8, 1.7.9, 6.1.2.2 and 4.4.3.1 are Level 1 workstation, 2.1.11 is Level 2 workstation
- tasks/section_6/cis_6.1.3.x.yml 6.1.3.6: now deploys the benchmark drop-in `/etc/rsyslog.d/60-rsyslog.conf` from the existing (previously unused) template instead of appending legacy `*.* @@host` syntax to `/etc/rsyslog.conf`
- tasks/section_6/cis_6.1.2.x.yml 6.1.2.1.4: journal-remote mask used hardcoded `state`/`enabled`, which fails when the unit is absent - now guarded by the package-presence ternary
- tasks/section_2/cis_2.3.2.x.yml 2.3.2.2: chrony and ntp masks were gated on package presence with `when`, losing the mask when the package was absent - now use the ternary guard; added the missing `patch` tag
- tasks/section_5/cis_5.3.3.2.x.yml and tasks/section_6/cis_6.2.4.x.yml: two untagged prep tasks registered variables consumed by tagged controls, so tagged runs failed on an undefined register
- tasks/remount_tmp.yml: `listen` on the `import_tasks` handler never reached the imported tasks, so /tmp never received nodev, nosuid or noexec - tasks flattened, each carrying its own `listen`
- tasks/section_6/cis_6.1.4.1.yml: the btmp/utmp/wtmp/lastlog task ANDed four mutually exclusive tests, so those files never had their permissions set
- tasks/section_6/cis_6.2.4.x.yml: 6.2.4.1 applied `u-x` recursively, leaving `/var/log/audit` as `drw-r-----` - now sets the mode on files only
- tasks/section_5/cis_5.4.2.x.yml: 5.4.2.8 lacked `create_home: false`, so the user module recreated tmpfs homes and reported changed on every run
- tasks/section_5/cis_5.4.1.x.yml: 5.4.1.5 matched every account with `$7~/(\s*|-1)/` and flagged values below the target, re-setting compliant accounts every run
- tasks/section_6/cis_6.3.x.yml: 6.3.2 disabled a static unit and forced a daemon reload every run - the cron branch now masks the dailyaidecheck units
- tasks/section_4/cis_4.2.x.yml: the optional `IPT_SYSCTL` task was tagged `always`, so it ran in tag-limited runs that never install ufw and failed on the missing `/etc/default/ufw` - now carries the section tags
- tasks/main.yml: `version_compare` replaced with the canonical `version` test
- tasks/section_5/cis_5.3.2.x.yml: `register` declared before `changed_when` in 8 tasks
- tasks/section_1/cis_1.4.x.yml and tasks/section_5/cis_5.1.x.yml: absolute symbolic modes replaced with relative
- templates/lockdown_audit.yml.j2: `ubtu24cis_dconf_db_name` and `ubtu24cis_warning_banner` were hardcoded rather than rendered from role values; added `ubtu24cis_screensaver_idle_delay` and `ubtu24cis_screensaver_lock_delay` so 1.7.4 and 1.7.5 can be audited
- templates/fs_with_cves.sh.j2: added the managed-by-ansible header
- Title separators corrected in 1.5.4, 5.1.4 and 5.4.1.5
- Removed two commented-out task blocks in section 4 (iptables persistence, now handled by handlers)
- issues collecting privilege comamnds for audit thanks to @dderemiah
- README.md: emoticons removed

### Community reported

- #183 thanks to @zac90 - 6.2.4.1-4 logfile discovery aborted the play when auditd was absent, and produced an empty path when auditd.conf had no `log_file` line
- #188 thanks to @golflimaechoecho, fixed in PR #189 - pam-configs templates carried a header `pam-auth-update` cannot parse, so it skipped the profiles
- #178 thanks to @alexmroke, reproduced by @bykvaadm - /etc/grub.d/00_user lost its executable bit, so the bootloader password never reached grub.cfg
- #182 thanks to @zac90 - the 5.3.3.2.x prep task had no tags, so tagged runs failed on an undefined register
- #185 thanks to @jbruno, workaround from @r2zer0-xsystem - `audit_file_git` and `audit_git_version` sat in `vars/`, so inventory could not override them
- #177 thanks to @bykvaadm - documented that Docker 29+ hard-depends on nftables, so 4.4.1.2 removes docker-ce with it
- #158 thanks to @seven-beep - documented the AppArmor profile conflict and the variables that opt out of it

## Based on CIS v1.0.0 - July 2026

### Fixed

- tasks/section_2/cis_2.1.x.yml line 642: Corrected service name typo `ngnix.service` to `nginx.service`
- defaults/main.yml: Set `audit_run_heavy_tests` default to `false` (opt-in convention — avoids slow tests in CI by default)
- defaults/main.yml: Moved 7 internal audit constants to `vars/audit.yml` where they belong (`audit_max_concurrent`, `get_audit_binary_method`, `audit_bin_copy_location`, `audit_content`, `audit_conf_source`, `audit_conf_dest`, `audit_log_dir`) — these are role implementation details that should not be user-overridable
- vars/CIS.yml (audit repo): Corrected `benchmark_version` from `2.0.0` to `1.0.0`
- molecule/localhost/converge.yml, molecule/wsl/converge.yml: Added `setup_audit: true` and `run_audit: true` so molecule runs the full audit on converge
- .github/workflows/export_badges_public.yml: Removed — this workflow is for public mirrors only and must not be present in a private repo
- CONTRIBUTING.md: Added — was missing from remediation repo
- 15 manual controls: Changed tag from `patch` to `audit` — these controls require manual intervention and must not perform active remediation: 1.1.1.10, 1.2.2.1, 3.1.1, 4.2.5, 4.4.2.3, 4.4.3.3, 5.3.3.2.3, 5.4.1.2, 6.1.1.3, 6.1.2.1.2, 6.1.3.5, 6.1.3.6, 6.1.3.8, 6.2.3.21
- updated 1.1.1.10 to be able to set location the script runs from
- ability to change default shell for shell module calls
- Converted ansible dot-notation references to bracket notation
- improved 5.2.4 tests and prelim check
- aligned audit variable naming wih remediation vars
- removed unneeded handlers
- tidy up audit variables
- Thanks to @bykvaadm
  - #157 PAM update
  - #168 sshd fix
  - #169 bootloader file permissions also thanks to @alexmroke
  - #176 dot file. excludion addition 7.2.10
- Thanks to seven-beep
  - #171 os_check addition
- thanks to @dderemiah
  - #167 pam improvements
- thanks to @hackery
  - #175 overlay mod change 1.1.1.6
- thanks to @defnotyujine
  - tmp mount handler changed to import_tasks
- 100 task files: Converted `ansible_facts` dot notation to bracket notation throughout `tasks/` and `vars/`
- Added `set -o pipefail` and `args: executable: /bin/bash` to tasks with pipes
- templates/ansible_vars_goss.yml.j2: Renamed to `templates/lockdown_audit.yml.j2`  updated reference in `tasks/pre_remediation_audit.yml`

## Based on CIS v1.0.0 - Branch [2026_April_QA]

### QA Validation

Molecule Results: Converge PASSED (ok=151, changed=7, failed=0), Verify PASSED (audit: 122 failures)

#### Fixed (Pre-QA Checks)

- vars/CIS.yml (audit): Fixed benchmark_version from '2.0.0' to '1.0.0'
- README.md: Removed RHEL dependencies (python-def, libselinux-python), updated to Python3.12+/Ansible 2.16+
- .gitignore: Added secret file patterns and QA artifact patterns
- tasks/main.yml: Added `community.docker.docker` to container connection detection

#### Changed (Standards Alignment)

- 48 shell tasks: Added `set -o pipefail` with `args: executable: /bin/bash`
- cis_2.1.x.yml, cis_3.1.x.yml: Applied package-aware ternary masking to 21 systemd mask tasks
- 62 discovery tasks: Added `check_mode: false`
- 44 discovery tasks: Specific `failed_when: <var>.rc not in [0, 1]` replacing broad `failed_when: false`
- 5 tasks: Fixed absolute mode notation to relative
- 12 tasks: Converted single-item `when:`/`notify:` to inline
- 80 loop tasks: Added `loop_control: label`

#### Security

- 7 tasks: Added `no_log: true` to `/etc/shadow` tasks

#### Fixed (Molecule Findings)

- vars/is_container.yml: Added missing `ubtu24cis_rule_6_2_2_4`
- 3 escaped quotes: Fixed after pipefail conversion
- 7 pwck tasks: Reverted to `failed_when: false` (SIGPIPE rc=141)

#### Added

- molecule/default/: Docker test scenario for Ubuntu 24.04 with audit verification
- molecule/localhost/: Delegated local test scenario
- molecule/wsl/: WSL delegated test scenario

#### Title Alignment

- 92 task titles: Updated to match CIS Ubuntu Linux 24.04 LTS Benchmark v1.0.0 titles exactly

#### Additional Fixes

- defaults/main.yml: Aligned header structure with UB22 standard — added role identification, variable precedence warning, `system_is_container`, UID discovery variables, `system_is_ec2`, `skip_for_test`. Removed duplicate variables.
- meta/main.yml, vars/main.yml: Updated `min_ansible_version` from 2.12.1 to 2.16.1
- vars/main.yml: Fixed `company_title` casing — `Mindpoint` → `MindPoint`

#### Issue Resolution

- tasks/prelim.yml: Fixed mount option gathering to preserve `/etc/fstab` source entries (UUID=, PARTUUID=, LABEL=) instead of using volatile `/dev/sdX` device names from `mount` output — addresses #162, thank you @samicemalone
- defaults/main.yml: Added commented `sntrup761x25519-sha512` post-quantum KEX algorithm option — addresses #156, thank you @cagriersen
- templates/tmp.mount.j2: Fixed systemd mount unit syntax `Options:` → `Options=` — addresses PR #163, thank you @kevingunn-wk
- defaults/main.yml: Added `ubtu24cis_tmp_partition_mount_options` variable for tmp.mount systemd unit — addresses PR #163

#### QA Results

- Rule coverage: 312/312 (100%)
- 0 duplicate registers, 0 bare FQCN, 0 absolute modes, 0 missing pipefail
- Cross-repo validator: info-level path differences only

---

## Based on CIS v1.0.0

# 2026 issue fixes
.gitignore update
lint and variable naming

Thanks to @bykvaadm
- #141 - 2.1.2 damon name
- #142 - ssh keys typo fix
- #143 - remove extended permissions
- #144 - tighten permissions on faillock
- #145 - 5.3.3.1.3 aligned command with CIS benchmark
- #146 - 5.3.3.3.x aligned
- #147 - 5.3.2 and 5.3.3.3.x pwhistory generation tidy up
- #149 - tidy up journald params
- #150 - 6.2.4.9 remove group from task
- #153 - 2.4.1.5 ability to add cron users

# 2026 Feb QA updates Benchmark 1.0.0
- Repo Checker QA fixes
- Grammar fixes: removed multiple consecutive spaces across defaults, tasks, and templates
- Grammar fixes: corrected repeated words ('is', 'the the', 'of', 'can must')
- Grammar fixes: fixed subject-verb disagreements in comments
- Added missing variable ubtu24cis_priv_command_excluded_mounts
- Added missing rule definition ubtu24cis_rule_4_4_1_4
- Updated audit URL reference from RHEL8 to UBUNTU24
- workflow updates
- company name alignments
- date updates
- lint improvements
- thanks to @bykvaadm
  - #136
  - #138
  - #139
- #137 thanks to @tmeckel

# 2026 Jan update

- 5.4.1.1/2/3 updated logic for ansible user based on #127 @rronneburger
- 5.4.2.7 added create_home false thanks to @seven-beep
- 5.4.3.2 improvement thanks tio @stelucz
- profile template now uses bash_umask variable
- audit template var updates improved logic
- 7.2.7 fixed typo in warning
- improved comments and description in defaults/main.yml

### 1.0.5 - based on benchmark CIS 1.0.0
precommit update
Public issues address
updated 4.2.5 for ntp access and improve loop variables

Many thanks to @DianaMariaDDM for the following:
#109 6.3.1 - Enhancements
#110 6.2.3.6 - improvement to privilege command collection
#111 6.3.2 - template for service fixed
#112 7.1.2 - Enhancements
#113 variable documentation tidy up
#119 tidy up typos
#120 3.3.3.1/5/8 - fix variables used
#121 6.1.1.4 added missing control
#122 separate 6.2.4.1/2/3
#124 removed unused template


### 1.0.5 - based on benchmark CIS 1.0.0
Public issues address
#92 1.1.1.7 logic improved and updating inline with audit branches - thanks @jbruno
#93 ufw logic improved thanks to @ToonSpinTUe
#94 Fixed var names dailychecktimer thanks to #94 @huan086
pre-commit udpates
typo fixes

### 1.0.4 - based on Benchmark CIS 1.0.0
pre-commit updates
workflow updates
Enabled for ansible 2.19
lint an alignment
tidy up spacing 1.3.1.3
max-concurrent added to auditd options - RTD also updated
5.3.3.4.2 reverted to pam_unix path
updated default name for pam_unix file

Issue Fixed:
thanks to @dderemiah
- issue #59

thanks to @dvic
- issues #62

thanks to @matt-j-griffin
- 60
- 63

thanks to @piotr1212
- 65

thanks to @ericwong3
- 71

thanks to @golflimaechoecho
- 75

thanks to @huan086
- 76
- 77

### 1.0.3 - based on Benchmark CIS 1.0.0
pre-commit updates
password data variable update - 7.2.10
removed +x from 5.4.2.6

Issue Fixed:
thanks to @ShawnHardwick
#47

thanks to @matt-j-griffin
#54

### 1.0.2 - based on Benchmark CIS 1.0.0

Linting
pre-commit updates
ability to fetch audit output

Issues fixed:
#21
#30
#31
#33
#34
#35
#41

### 1.0.1 - based on Benchmark CIS 1.0.0

ARM64 now working with auditd
pre-commit updates
linting
many updates

Issues fixed:
#9
#10
#12
#15
#18
#19
#20

### 1.0.0 - based on Benchmark CIS 1.0.0
Initial
