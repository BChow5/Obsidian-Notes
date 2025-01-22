- Timers are [systemd](https://wiki.archlinux.org/title/Systemd "Systemd") unit files whose name ends in _.timer_ that control _.service_ files or events. 
- Timers can be used as an alternative to [cron](https://wiki.archlinux.org/title/Cron "Cron") (read [#As a cron replacement](https://wiki.archlinux.org/title/Systemd/Timers#As_a_cron_replacement)). 
- Timers have built-in support for calendar time events, monotonic time events, and can be run asynchronously.
- 
### Timer units

Timers are _systemd_ unit files with a suffix of _.timer_. Timers are like other [unit configuration files](https://wiki.archlinux.org/title/Systemd#Writing_unit_files "Systemd") and are loaded from the same paths but include a `[Timer]` section which defines when and how the timer activates. Timers are defined as one of two types:

- **Realtime timers** (a.k.a. wallclock timers) activate on a calendar event, the same way that cronjobs do. The option `OnCalendar=` is used to define them.
- **Monotonic timers** activate after a time span relative to a varying starting point. They stop if the computer is temporarily suspended or shut down. There are number of different monotonic timers but all have the form: `On_Type_Sec=`. Common monotonic timers include `OnBootSec` and `OnUnitActiveSec`.

For a full explanation of timer options, see the [systemd.timer(5)](https://man.archlinux.org/man/systemd.timer.5). The argument syntax for calendar events and time spans is defined in [systemd.time(7)](https://man.archlinux.org/man/systemd.time.7).

**Note:** _systemd_ offers the target `timers.target` which sets up all timers that should be active after boot (see [systemd.special(7)](https://man.archlinux.org/man/systemd.special.7) for details). To use it, add `WantedBy=timers.target` to the `[Install]` section of your timer and [enable](https://wiki.archlinux.org/title/Enable "Enable") the timer unit.

### Service units

- For each _.timer_ file, a matching _.service_ file exists (e.g. `foo.timer` and `foo.service`). 
	- The _.timer_ file activates and controls the _.service_ file. 
- The _.service_ does not require an `[Install]` section as it is the _timer_ units that are enabled. 
- If necessary, it is possible to control a differently-named unit using the `Unit=` option in the timer's `[Timer]` section.

### Management

To use a _timer_ unit [enable](https://wiki.archlinux.org/title/Enable "Enable") and [start](https://wiki.archlinux.org/title/Start "Start") it like any other unit (remember to add the _.timer_ suffix). To view all started timers, run:

![[Pasted image 20241111192452.png]]

**Note:**
- To list all timers (including inactive), use `systemctl list-timers --all`.
- The status of a service started by a timer will likely be inactive unless it is currently being triggered.
- If a timer gets out of sync, it may help to delete its `stamp-*` file in `/var/lib/systemd/timers` (or `~/.local/share/systemd/` in case of user timers). These are zero length files which mark the last time each timer was run. If deleted, they will be reconstructed on the next start of their timer.

### Examples

A service unit file can be scheduled with a timer out-of-the-box. The following examples schedule `foo.service` to be run with a corresponding timer called `foo.timer`.

#### Monotonic timer

A timer which will start 15 minutes after boot and again every week while the system is running.

![[Pasted image 20241111192618.png]]

#### Realtime timer

A timer which starts once a week (at 12:00am on Monday). When activated, it triggers the service immediately if it missed the last start time (option `Persistent=true`), for example due to the system being powered off:
![[Pasted image 20241111192700.png]]

When more specific dates and times are required, `OnCalendar` events uses the following format:

`DayOfWeek Year-Month-Day Hour:Minute:Second`

An asterisk may be used to specify any value and commas may be used to list possible values. Two values separated by `..` indicate a contiguous range.

In the below example the service is run the first four days of each month at 12:00 PM, but _only_ if that day is a Monday or a Tuesday.

`OnCalendar=Mon,Tue *-*-01..04 12:00:00`

To run a service on the first Saturday of every month, use:

`OnCalendar=Sat *-*-1..7 18:00:00`

When using the `DayOfWeek` part, at least one weekday has to be specified. If you want something to run every day at 4am, use:

`OnCalendar=*-*-* 4:00:00`

To run a service at different times, `OnCalendar` may be specified more than once. In the example below, the service runs at 22:30 on weekdays and at 20:00 on weekends.

`OnCalendar=Mon..Fri 22:30`
`OnCalendar=Sat,Sun 20:00`

You can also specify a timezone at the end of the directive (use `timedatectl list-timezones` to list accepted values)

`OnCalendar=*-*-* 02:00:00 Europe/Paris`

**Tip:**

- `OnCalendar` time specifications can be tested in order to verify their validity and to calculate the next time the condition would elapse when used on a timer unit file with the `calendar` option of the _systemd-analyze_ utility. For example, one can use `systemd-analyze calendar weekly` or `systemd-analyze calendar "Mon,Tue *-*-01..04 12:00:00"`. Add `--iterations=N` to ask for more iterations to be printed.
- The `faketime` command is especially useful to test various scenarios with the above command; it comes with the [libfaketime](https://archlinux.org/packages/?name=libfaketime) package.
- Special event expressions like `daily` and `weekly` refer to _specific start times_ and thus any timers sharing such calendar events will start simultaneously. Timers sharing start events can cause poor system performance if the timers' services compete for system resources. The `RandomizedDelaySec` option in the `[Timer]` section avoids this problem by randomly staggering the start time of each timer. See [systemd.timer(5)](https://man.archlinux.org/man/systemd.timer.5).
- Add the option `AccuracySec=1us` to the `[Timer]` section, to avoid the inaccuracy of the _1m_ default value of `AccuracySec`. Also see [systemd.timer(5)](https://man.archlinux.org/man/systemd.timer.5).
- Some options (`WakeSystem`) may require specific system capabilities and prevent a timer from starting, resulting in the following error messages: "Failed to enter waiting state: Operation not supported" and "Failed with result 'resources'.".

### Transient timer units

One can use `systemd-run` to create transient _.timer_ units. That is, one can set a command to run at a specified time without having a service file. For example the following command touches a file after 30 seconds:

`# systemd-run --on-active=30 /bin/touch /tmp/foo`

One can also specify a pre-existing service file that does not have a timer file. For example, the following starts the systemd unit named `_someunit_.service` after 12.5 hours have elapsed:

`# systemd-run --on-active="12h 30m" --unit _someunit_.service`

See [systemd-run(1)](https://man.archlinux.org/man/systemd-run.1) for more information and examples.

### As a cron replacement

Although [cron](https://wiki.archlinux.org/title/Cron "Cron") is arguably the most well-known job scheduler, _systemd_ timers can be an alternative.

#### Benefits

The main benefits of using timers come from each job having its own _systemd_ service. Some of these benefits are:

- Jobs can be easily started independently of their timers. This simplifies debugging.
- Each job can be configured to run in a specific environment (see [systemd.exec(5)](https://man.archlinux.org/man/systemd.exec.5)).
- Jobs can be attached to [cgroups](https://wiki.archlinux.org/title/Cgroups "Cgroups").
- Jobs can be set up to depend on other _systemd_ units.
- Jobs are logged in the _systemd_ journal for easy debugging.

#### Caveats

Some things that are easy to do with cron are difficult to do with timer units alone:

- Creation: to set up a timed job with _systemd_ you need to create two files and run `systemctl` commands, compared to adding a single line to a crontab.
- Emails: there is no built-in equivalent to cron's `MAILTO` for sending emails on job failure. See [#MAILTO](https://wiki.archlinux.org/title/Systemd/Timers#MAILTO) for an example of setting up a similar functionality using `OnFailure=`.

Also note that [user](https://wiki.archlinux.org/title/Systemd/User "Systemd/User") timer units will only run during an active user login session by default. However, [lingering](https://wiki.archlinux.org/title/Systemd/User#Automatic_start-up_of_systemd_user_instances "Systemd/User") can enable services to run at boot even when the user has no active login session.

### Handling "time to live"

Some software will track the time elapsed since they last ran, for example blocking the update of a database if the last download ended less than 24 hours ago.

By default, timers do not track when the task they launched has ended. To work around this, we can use `OnUnitInactiveSeconds`:
![[Pasted image 20241111194059.png]]
**Tip:** With `Restart=on-failure` along with `RestartSec`, it is possible to have a unit rerun after failure and success according to different schedules, see [systemd.service(5) § OPTIONS](https://man.archlinux.org/man/systemd.service.5#OPTIONS).