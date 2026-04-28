# Ubuntu24CIS

## Based on CIS v1.0.0 - Branch [2026_April_QA]

### QA Validation

**Molecule Results:** Converge PASSED (ok=151, changed=7, failed=0), Verify PASSED (audit: 122 failures)

#### Fixed (Pre-QA Checks)

- **vars/CIS.yml (audit):** Fixed benchmark_version from '2.0.0' to '1.0.0'
- **README.md:** Removed RHEL dependencies (python-def, libselinux-python), updated to Python3.12+/Ansible 2.16+
- **.gitignore:** Added secret file patterns and QA artifact patterns
- **tasks/main.yml:** Added `community.docker.docker` to container connection detection

#### Changed (Standards Alignment)

- **48 shell tasks:** Added `set -o pipefail` with `args: executable: /bin/bash`
- **cis_2.1.x.yml, cis_3.1.x.yml:** Applied package-aware ternary masking to 21 systemd mask tasks
- **62 discovery tasks:** Added `check_mode: false`
- **44 discovery tasks:** Specific `failed_when: <var>.rc not in [0, 1]` replacing broad `failed_when: false`
- **5 tasks:** Fixed absolute mode notation to relative
- **12 tasks:** Converted single-item `when:`/`notify:` to inline
- **80 loop tasks:** Added `loop_control: label`

#### Security

- **7 tasks:** Added `no_log: true` to `/etc/shadow` tasks

#### Fixed (Molecule Findings)

- **vars/is_container.yml:** Added missing `ubtu24cis_rule_6_2_2_4`
- **3 escaped quotes:** Fixed after pipefail conversion
- **7 pwck tasks:** Reverted to `failed_when: false` (SIGPIPE rc=141)

#### Added

- **molecule/default/:** Docker test scenario for Ubuntu 24.04 with audit verification
- **molecule/localhost/:** Delegated local test scenario
- **molecule/wsl/:** WSL delegated test scenario

#### Title Alignment

- **92 task titles:** Updated to match CIS Ubuntu Linux 24.04 LTS Benchmark v1.0.0 titles exactly

#### Additional Fixes

- **defaults/main.yml:** Aligned header structure with UB22 standard — added role identification, variable precedence warning, `system_is_container`, UID discovery variables, `system_is_ec2`, `skip_for_test`. Removed duplicate variables.
- **meta/main.yml, vars/main.yml:** Updated `min_ansible_version` from 2.12.1 to 2.16.1
- **vars/main.yml:** Fixed `company_title` casing — `Mindpoint` → `MindPoint`

#### Issue Resolution

- **tasks/prelim.yml:** Fixed mount option gathering to preserve `/etc/fstab` source entries (UUID=, PARTUUID=, LABEL=) instead of using volatile `/dev/sdX` device names from `mount` output — addresses #162, thank you @samicemalone
- **defaults/main.yml:** Added commented `sntrup761x25519-sha512` post-quantum KEX algorithm option — addresses #156, thank you @cagriersen
- **templates/tmp.mount.j2:** Fixed systemd mount unit syntax `Options:` → `Options=` — addresses PR #163, thank you @kevingunn-wk
- **defaults/main.yml:** Added `ubtu24cis_tmp_partition_mount_options` variable for tmp.mount systemd unit — addresses PR #163

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
