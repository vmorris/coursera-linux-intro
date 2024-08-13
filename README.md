coursera

# Hands-on Introduction to Linux Commands and Shell Scripting

## Module 1: Intro to Linux
https://www.coursera.org/learn/hands-on-introduction-to-linux-commands-and-shell-scripting/home/module/1

Hands-on Labs:

- Getting Started with the Linux Terminal
- Installing and working with text editors

## Module 2: Intro to Linux Commands
https://www.coursera.org/learn/hands-on-introduction-to-linux-commands-and-shell-scripting/home/module/2

Hands-on Labs:

- Informational Commands
- Navigating and Managing Files and Directories
- Access Control Commands
- Wrangling Text Files at the Command Line
- Working with Networking Commands
- Archiving and Compressing Files

## Module 3: Introduction to Shell Scripting
https://www.coursera.org/learn/hands-on-introduction-to-linux-commands-and-shell-scripting/home/module/3

Hands-on Labs:

- Getting Started with Shell Scripting
- Advanced Bash Scripting
- Scheduling Jobs using Crontab

Basic building blocks of programming are:

1. Sequence: statements run in order, one after another (do x y z done)
2. Repetition: blocks of statements can be iterated, or looped (while, for, until)
3. Branching: logical flow controls through conditional statement evaluation (if, then, else)

## Module 4: Final Project and Final Exam
https://www.coursera.org/learn/hands-on-introduction-to-linux-commands-and-shell-scripting/home/module/4

### Practice Project

Hands-on Labs:

- Historical Weather Forcast Comparison to Actuals

### Final Project

Hands-on Labs:

- Peer-Graded Final Project


---

## Personal takes on bash scripting:

### pros:

- document linux commands to do something specific
- automation
- runs on all linux

### cons:

- technical debt
- not ideal for applications
- some difficult syntax

I prefer Python for most scripting needs!

---

## Example bash scripts I've written

### Run a command, save the output, loop through the output

```bash
#!/usr/bin/env bash
# Shutdown and restart all nodes in OCP cluster
nodes=$(oc get nodes -o jsonpath='{.items[*].metadata.name}')
for node in ${nodes[@]}; do
    echo "==== Restart $node ===="
    ssh core@$node sudo shutdown -r now
done
```

### Use a simple array to SSH in a loop

```bash
#!/usr/bin/env bash
# Check the hostname and date on multiple machines
servers=( "10.0.0.1"
          "10.0.0.2"
          "10.0.0.3" )
for server in ${servers[@]}; do
    ssh $server "hostname; date"
done
```

### Loop through the output of a command and run another command

```bash
#!/usr/bin/env bash
# Get a list of 
for proj in $(openstack --os-cloud cic-admin project list -f json | jq -r .[].ID); do
    echo "Project: $proj"
    openstack --os-cloud cic-admin --os-project-id $proj volume list -f csv
done
```

### Clean up a deployed environment

```bash
#!/usr/bin/env bash
DEL_ARTIFACTS=No
while true; do
    read -p "Do you want to delete the installation artifacts (deployment directory)? (yes/no) " yn
    case $yn in
        [Yy]* ) DEL_ARTIFACTS=Yes; break;;
        [Nn]* ) DEL_ARTIFACTS=No; break;;
        * ) echo "Please answer yes or no.";;
    esac
done
if [[ $DEL_ARTIFACTS == "Yes" ]]; then
    rm -rf deployment
    rm services_inventory.yaml
fi
ansible-playbook teardown.yaml
```

### Check the last exit code for some error and handle it

```bash
function check_rc {
        local func_rc=$?
        if [[ $func_rc -ne 0 ]] ; then
                echo "ERROR: Exit code was $func_rc, exiting!"
                exit
        fi
}
```

