# Ubuntu24CIS

## CIS Benchmark 2.0.0

## November 2026 - QA fixes

- audit binary switched from goss to syver v0.16.0
- prelim_goss_available renamed prelim_audit_bin_available
- README audit section describes syver
- optional syver release signature check with gpgv, off by default (audit_bin_verify_signature)
- audit file renamed syver.yml, goss_template tag renamed syver_template
- audit_system_type passed to run_audit.sh (-w for Workstation)
- audit summary reports "not run in check mode" under --check
- /tmp management auto discovered (ubtu24cis_tmp_svc: auto), bridged to the audit
- audit bridge: ubtu24_legacy_boot renamed ubtu24cis_legacy_boot
- playbook user password check matches the exact account name (addresses https://github.com/ansible-lockdown/UBUNTU24-CIS/issues/204) - Thank you @dan-ostermeier
- 5.2.4 prelim password and lock checks skip users in ubtu24cis_sudoers_exclude_nopasswd_list (port of https://github.com/ansible-lockdown/RHEL10-CIS/issues/129)
- pam-auth-update handlers flushed after 5.3.x
- 5.4.1.x ansible_env replaced by ansible_facts['env']
- 5.4.1.6 never expires the connecting user's password
- set -o pipefail on every shell task, args executable on 3.1.2
- auditd stop/start handlers skipped in containers; immutable auditd rule change flags a reboot
- README support link anchors consistent
- 1.2.1.4, 1.2.1.5, 1.2.1.7, 1.2.1.8 directory modes symbolic, never loosen
- 6.1.2.9, 6.1.2.10 forward config mode symbolic
- 5.4.2.x and 7.2.x interactive users exclude /usr/bin/false and any nologin path
- interactive user filter matches whole account names
- 5.4.2.8 compares account names against the interactive user list, not a substring
- 7.2.2 locks accounts without creating a home directory
- 2.1.23 keeps xserver-common when ubtu24cis_desktop_required is true (as UBUNTU26-CIS issue https://github.com/ansible-lockdown/UBUNTU26-CIS/issues/3) - Thank you @sam-stenvall-iceye
- 6.1.2.4 rsyslog config lookup returns a single path
- 3.3.1.14, 3.3.1.15, 3.3.2.5, 3.3.2.6 applied on routers; 3.3.2.3, 3.3.2.4 skipped on routers
- README rendered from the canonical template
- ubtu24cis_sudoers_exclude_nopasswd_list passed to the audit
- workflow actions pinned to release tags, actions.lock and dependabot added
- 2.1.6: nginx-plus removed with nginx
- 5.4.2.7: playbook user and ubtu24cis_system_users_shell accounts keep their shell
- 5.4.3.3: only umask values weaker than 027 changed, /etc/bash.bashrc included
- 5.4.2.2: GID 0 account names matched exactly
- 6.1.3.1: symlinked log files followed, link state accepted
- 7.2.9: ACLs skip home directories that do not exist
- update-grub handler failures no longer hidden
- bridge passes ubtu24cis_system_users_shell and ubtu24cis_playbook_user

## October 2026 - QA fixes

- CIS Benchmark 1.0.0 October 2026 QA fixes merged and ported to 2.0.0 control IDs
- 1.3.1.3 aa-enforce run through the shell so the glob expands
- 3.2.1 and 3.2.2 modprobe files get both install and blacklist lines
- 6.1.1.1.4 no longer changes the vendor tmpfiles file
- 6.1.1.1.4 override check reads /etc/tmpfiles.d
- 6.1.1.1.5 to 6.1.1.1.7 journald restart notified per task
- 6.1.2.4 emerg lookup limited to rsyslog config
- 6.1.2.5 and 6.1.2.9 skipped until ubtu24cis_remote_log_server is set
- 6.1.2.6 imtcp regexps escaped
- 6.1.3.1 login record and sssd log permissions loop fixed
- 6.1.1.1.4 and 6.2.3.30 tasks named AUDIT
- automated and manual tags set from the 2.0.0 benchmark
- unused systemd-journal-upload handler removed
- README AppArmor note updated for 2.0.0 control IDs
- 1.2.1.1 checks apt sources for Signed-By
- 1.2.1.2 disables apt weak dependencies, level 2
- 1.6.4 sets pam_motd file content; dynamic motd lines commented out in sshd, login, su and gdm-password
- 2.1.5 removes kea and masks the kea-dhcp services
- 5.1.23 adds post-quantum KexAlgorithms chosen by the OpenSSH server version
- 5.1.12 and 5.1.23 write the same KexAlgorithms line
- 5.4.2.6 sets umask in /root/.profile and /root/.bashrc
- 5.4.2.7 sets nologin on system accounts with a valid shell
- 6.3.2 uses dailyaidecheck.timer only, cron option removed
- ubtu24cis_aide_cron_* schedule variables renamed ubtu24cis_aide_timer_*
- dailyaidecheck.timer OnCalendar built in systemd order
- dailyaidecheck.service is static, started only by the timer
- 1.2.1.3 sets owner and mode on apt gpg key files
- 1.5.4 sets only fs.suid_dumpable
- 1.5.11 and 1.5.12 use a coredump.conf.d drop-in with a [Coredump] section
- 1.6.10 uses ubtu24cis_sshd_banner_file
- 1.7.x write /etc/dconf/profile/user, 1.7.1 and 1.7.2 lock files added
- kernel module controls install /bin/false, 3.2.x unload the module
- 2.1.19 masks inetutils-inetd.service
- 2.2.1 uses ubtu24cis_nis_client_required
- 2.3.2.2 and 2.4.1.1 unmask before start
- 2.4.2.1 at.allow group daemon when present
- 3.3.x conflicting sysctl entries commented out, package files warned
- 5.1.x sshd settings in sshd_config.d/00-cis-sshd.conf, Include guard added
- 5.1.x CIS keywords above the Include line commented out
- 5.1.24 old ListenAddress lines commented out
- 5.2.2 removes !use_pty
- 5.2.6 replaces only negative or over 15 timestamp_timeout values
- 5.3.2.1 enables pam unix without the template toggle
- 5.3.3.1.x faillock arguments stripped wherever they sit, root_unlock_time under 60 removed
- 5.3.3.2.x conf.d directory created for 5.3.3.2.7 and 5.3.3.2.8
- 5.3.3.3.x pwhistory arguments set in the pam-configs profile
- 5.3.3.4.2 to 5.3.3.4.4 edit the unix pam-configs profile
- 5.4.2.1 to 5.4.2.3 audit and warn only
- 5.4.3.1 removes whole nologin lines
- 5.4.3.3 four-digit umask kept, weak profile.d umask lines commented out
- 6.1.1.1.2, 6.1.1.1.6 and 6.1.1.1.7 no longer gated on journald
- 6.1.1.1.3 ForwardToSyslog drop-in, journald restarted
- 6.1.2.2 rsyslog unmasked and started
- 6.1.2.6 comments out only imtcp lines, rsyslog.d included
- 6.1.3.1 log file owner and group corrected
- 6.2.3.4 clock_settime a0=0x0 rules added
- 6.2.3.22 renameat2 added
- 6.2.3.28 b32 kernel module rule added
- 6.2.4.4 log_group adm set
- 7.2.1 runs pwconv when passwords are not shadowed
- 7.2.4 removes shadow group members
- stale v1 firewall notes removed


## Based on CIS v2.0.0 - August 2026

### Benchmark upgrade v1.0.0 -> v2.0.0

- 312 -> 333 controls: 71 added, 50 retired, 80 renumbered, titles resynced from the v2.0.0 benchmark
- Section 3.3 split into 3.3.1 (IPv4, 18 controls) and 3.3.2 (IPv6, 8 controls, gated on `ubtu24cis_ipv6_required`), one control per kernel parameter, replacing the 11 combined v1 controls
- Section 4 reduced to UFW only at 4.1.x - the nftables (4.3.x) and iptables (4.4.x) sections are dropped from the benchmark; `ubtu24cis_firewall_package` no longer selects between firewalls for these controls
- Section 6.1 renested: journald moves to 6.1.1.1.x, rsyslog to 6.1.2.x, logfiles to 6.1.3.1; the systemd-journal-remote/upload section is dropped
- Section 6.2.3 auditd rules split: the two combined v1 controls (system network environment, user/group information) become 6.2.3.5-6.2.3.9 and 6.2.3.11-6.2.3.16, each with its own toggle in `templates/audit/99_auditd.rules.j2`
- Sections 1.2.1 (apt key/source access), 1.5 (per-parameter process hardening), 1.6 (motd/issue/banner access) and 7.1 (system file access) expanded
- New variables: `ubtu24cis_sshd_banner_file`, `ubtu24cis_sshd_listen_address`, `ubtu24cis_rsyslog_ca_certificate`
- Removed variables left dead by retired controls: `ubtu24cis_apparmor_mode`, `control_1_3_1_4_was_run`, `ubtu24cis_apparmor_enforce_only`, and the three `ubtu24cis_journal_*certificate/keyfile` variables
- fixed the collection of audit privileged commands

### Fixed

- 13 controls carried level tags that disagreed with the v2.0.0 Profile Applicability after renumbering; the audit repo had 29 `meta.server`/`meta.workstation` values and 12 Jinja level gates with the same drift
- `vars/is_container.yml` disabled `ubtu24cis_rule_1_3_1` to `_1_3_4`, IDs this benchmark has never used, so the AppArmor controls were never actually skipped in containers - now `1_3_1_1` to `1_3_1_4`; also dropped `ubtu24cis_rule_6_2_4_11`, which does not exist
- `molecule/default/converge.yml` set `ubtu24cis_rule_1_3_1_3` twice (duplicate YAML key) after renumbering; the second is now `1_3_1_4`
- tasks/section_5/cis_5.2.x.yml: the sudoers file discovery task was tagged `rule_5.2.5`/`rule_5.2.6` but gated on `rule_5_2_4`/`rule_5_2_5`, so a tagged 5.2.5 run could skip the task that registers the file list it consumes
- handlers/main.yml: removed `Iptables persistent` and `Ip6tables persistent`, unreachable once section 4.4 was retired
- tasks/section_6/cis_6.1.1.1.x.yml: the shared journald conf.d task still gated on retired control 6.1.2.2, whose ID v2 reassigns to the rsyslog service check
- templates/lockdown_audit.yml.j2: the rule-toggle block had no `############` end marker, so the bridge template was never machine-maintained; added, and the block rebuilt to the v2 toggle set
- 1.7.3, 1.7.4 and 1.7.5 merge each GDM setting with its former "not overridden" partner in v2; the tasks now write the dconf lock file as well as the setting, which also brings the three `00-*_lock.j2` templates back into use
- templates/etc/systemd/journald.conf.d/forwardtosyslog.conf.j2 removed - it set `ForwardToSyslog=no` for retired control 6.1.2.2, the opposite of what v2 6.1.1.1.3 requires

## CIS Benchmark 1.0.0

## November 2026 - QA fixes

- playbook user password check matches the exact account name (addresses https://github.com/ansible-lockdown/UBUNTU24-CIS/issues/204) - Thank you @dan-ostermeier
- 5.4.1.x user lookups use ansible_facts['env']
- pam-auth-update handlers flushed after 5.3.x
- Stop and Start auditd handlers skipped in containers
- immutable auditd rule change now flags a reboot
- 1.1.1.10 discovery script checks all installed kernels and nested modules
- ubtu24cis_tmp_svc defaults to auto, /tmp management discovered in prelim
- ubtu24cis_tmp_svc passed to the audit
- 1.5.3 coredump.conf change reloads systemd
- 1.6.4 /etc/motd permissions set only when present
- 2.2.1 nis client gated on new ubtu24cis_nis_client_required
- 2.3.2.2 and 2.4.1.1 units unmasked before enable
- 2.4.2.1 at.allow group daemon when present
- 3.1.2 wireless modules set to /bin/false
- 3.2.1 to 3.2.4 loaded modules unloaded
- 4.1.1 active firewall count fixed
- 4.2.3 ufw service unmasked
- 4.4.3.2 to 4.4.3.4 run with 4.4.3.1, IPv6 ssh allowed in
- 4.4.2.2 INPUT loopback drop task name
- 5.2.2 !use_pty removed
- 5.2.6 only negative or over 15 timestamp_timeout replaced
- 5.3.3.1.x faillock regexps anchored, profile handler notified
- 5.3.3.2.7 and 5.3.3.2.8 create the pwquality.conf.d directory
- 5.3.3.3.x pwhistory regexps match the pam-configs profile
- 5.3.3.4.3 and 5.3.3.4.4 edit the pam-configs unix profile
- 5.4.1.6 skips the connection user
- 5.4.2.1 and 5.4.2.2 warn only, no account deleted
- 5.4.2.6 umask set in /root/.bash_profile and /root/.bashrc
- 5.4.2.7 nologin set on system accounts with a valid shell
- 5.4.3.1 nologin lines removed from /etc/shells
- 5.4.3.3 umask regexp anchored, weak profile.d umask commented out
- 6.1.1.4 journald stop task commented out
- 6.1.3.2 rsyslog unmasked and started
- 6.1.3.7 only imtcp lines changed, rsyslog.d included
- 6.1.4.1 log file owner and group set
- 6.2.3.4 clock_settime rule with a0=0x0
- 6.2.4.3 log_group = adm
- 6.3.2 timer OnCalendar order fixed, timer started not the service
- 7.1.11 world writable sticky bit no longer touches files
- 7.2.9 home directories created for interactive users only
- 7.2.10 dot file owner matched by exact home directory
- 1.1.1.6 and 1.1.1.9 module unload no longer fails when in use
- 1.5.3 coredump.conf settings under [Coredump], only when systemd-coredump installed
- 1.7.2 and 1.7.3 gdm dconf profile and gdm.d keyfiles
- 1.7.4 settings written to the dconf user profile
- 2.4.1.8 cron.allow group crontab when present
- 2.4.2.1 only when at installed
- 3.1.2 loaded wireless modules unloaded
- 5.3.3.4.2 remember removed from pam_unix in all pam-configs profiles
- 5.4.2.8 valid shells and usernames matched exactly
- 6.1.2.1.3 systemd-journal-upload started as well as enabled
- 6.3.2 packaged dailyaidecheck.timer used, cron option removed
- 6.3.2 timer schedule set by drop-in
- 6.3.2 cron jobs and unit overrides from earlier role runs removed
- 6.3.2 warns on other AIDE cron jobs
- ubtu24cis_aide_cron_* renamed ubtu24cis_aide_timer_*
- 7.2.1 pwconv migrates accounts without shadowed passwords
- 7.2.4 shadow group members removed, warn on shadow primary group
- audit bridge: ubtu24cis_sudo_package, ubtu24cis_coredump_installed, ubtu24cis_at_installed
- audit_system_type selects the Server or Workstation audit profile
- 5.2.4 prelim password and lock checks skip users in ubtu24cis_sudoers_exclude_nopasswd_list (port of https://github.com/ansible-lockdown/RHEL10-CIS/issues/129)
- audit summary capture skipped in check mode
- audit bridge: legacy_boot key uses the ubtu24cis_ prefix
- README tracking tags consistent
- audit bridge: ubtu24cis_apport_installed

## October 2026 - QA fixes

- 5.3.3.1.1 to 5.3.3.1.3 faillock regexps fixed
- 5.4.1.6 current date computed in a shell
- 7.2.2 and 5.4.2.2 accounts looped individually
- 7.2.8 checks group names
- 2.1.6 vsftpd package name corrected
- 5.2.2 sudoers edit validated with visudo
- 5.2.6 timestamp_timeout regexp fixed
- 6.1.3.7 imtcp regexps escape the parentheses
- 6.1.1.2 checks the /etc/tmpfiles.d override, vendor file left alone
- 6.1.2.1.2 and 6.1.3.6 forward logs only when ubtu24cis_remote_log_server is set, default empty
- ubtu24_ptrace_value renamed ubtu24cis_ptrace_value and passed to the audit
- README notes the AppArmor mode change made by 1.3.1.3 and 1.3.1.4
- molecule converge auditd overrides removed, is_container.yml covers them
- bridge template rule section end marker added
- route flush handlers use sysctl -w and no longer write /etc/sysctl.conf
- auditd user exclusions exclude the listed users, not everyone else
- 2.1.22 exim4 changes run update-exim4.conf
- 2.3.3.3 unmasks chrony before starting it
- 6.1.3.4 finds the rsyslog config in /etc/rsyslog.conf and rsyslog.d only
- 7.1.11 sticky bit step skips when no directories are found
- bridge emits ubtu24cis_varlog_location, matching the audit
- auditd rule file written 0640 so 6.2.4.5 does not change it on the next run
- 6.1.1.3 journald rotation defaults set to the benchmark example values
- 6.1.2.1.3 enables journal upload only when ubtu24cis_remote_log_server is set
- 1.3.1.3 and 1.3.1.4 apparmor profile glob now expands (addresses https://github.com/ansible-lockdown/UBUNTU24-CIS/issues/195) - Thank you @lnagelschmidt
- 1.3.1.3 and 1.3.1.4 fail on aa-complain and aa-enforce errors
- 6.2.1.1 package facts refreshed after the auditd install (addresses https://github.com/ansible-lockdown/UBUNTU24-CIS/issues/196) - Thank you @lan94
- auditd handlers tolerate a skipped immutable check or augenrules check
- 5.1.12 post-quantum KEX example uses the @openssh.com name, indented (addresses https://github.com/ansible-lockdown/UBUNTU24-CIS/issues/198) - Thank you @CyberNativeAI
- 1.4.1 00_user readable and executable by all (addresses https://github.com/ansible-lockdown/UBUNTU24-CIS/issues/178) - Thank you @alexmroke
- 1.1.2.1.1 to 1.1.2.7.1 missing partition warnings now fire
- 2.1.1 autofs mask runs when autofs is not installed
- 5.2.5 sudoers discovery gated on 5.2.5 only, rule_5.2.6 tag removed
- 5.3.2.1 to 5.3.2.4 pam discovery tasks run in check mode
- 6.1.4.1 login records and sssd logs task loops the discovered log files
- 7.2.7 and 7.2.8 duplicate name warnings use the registered variable
- 1.7.2, 1.7.3, 2.1.10, 6.1.1.3 and 6.1.2.2 to 6.1.2.4 notify moved off the block
- /tmp systemd handler enables and unmasks tmp.mount
- /tmp mount busy warning now fires
- duplicate Grub update handler removed
- 3.1.2 wireless discovery register renamed to discovered_wireless_adapters
- automated and manual tags added from the benchmark for 315 tasks
- ansible_local replaced with ansible_facts
- molecule verify play sets audit_log_dir and audit_format
- Company name updated to MindPoint Group - A Quantum Sky Company
- unused ubtu24cis_skip_for_test removed
- molecule scenarios run the role via roles:, read-only role_name var removed
- 6.1.2.2 ForwardToSyslog drop-in written as syslog.conf so the systemd vendor default no longer overrides it
- 1.1.2.1.x tmp.mount task name lists the right controls
- task name spacing fixed in 1.1.2.2.1, 1.1.2.7.1 and 5.1.5
- 6.2.3.21 named AUDIT
- defaults comments aligned to v1.0.0 control IDs
- 6.1.2.1.2 journal-upload.conf changes restart systemd-journal-upload
- ubtu24cis_disruption_high passed to the audit

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
