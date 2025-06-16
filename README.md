# Linux System Patching and Validation Playbook

This Ansible playbook automates the process of patching Linux systems, collecting pre- and post-update system information, validating services, filesystems, and network interfaces, and logging all actions for audit and troubleshooting purposes.

## Features
- Gathers and logs pre-update and post-update OS information
- Collects and logs service states, mounted filesystems, and network interface details
- Updates all packages to the latest versions using `apt`
- Handles kernel upgrades
- Logs patch status and update results
- Validates that services and filesystems are running/mounted after patching
- Checks that network interfaces remain active and retain their IP addresses
- Assembles and displays a final report from all log fragments

## Usage
1. **Set Required Variables:**
   - `remote_dir`: Path for remote logging (define in inventory or extra vars)
   - `activity_logs`: Directory for storing log files
   - `final_report`: Path for the assembled final report
   - `confirm_update`: Boolean to control if updates are applied

2. **Run the Playbook:**
   ```bash
   ansible-playbook linux.yml -e "activity_logs=/tmp/logs final_report=/tmp/final_report.txt confirm_update=true"
   ```

3. **Review Logs and Reports:**
   - Logs are created in the `activity_logs` directory
   - The final report is assembled at the path specified by `final_report`

## Requirements
- Ansible 2.9+
- Target hosts must be Linux systems with Python and `apt` package manager (Debian/Ubuntu)
- Sudo privileges for package and service management

## Customization
- To support other distributions (e.g., RHEL/CentOS), add conditionals for `yum` or `dnf` tasks
- Adjust logging paths and variables as needed for your environment
- Extend validation steps as required for your infrastructure

## Structure
- **Pre-update:** Gathers facts, logs system state
- **Patching:** Updates packages, logs results, handles errors
- **Post-update:** Validates services, filesystems, and interfaces, logs outcomes
- **Reporting:** Assembles and displays a comprehensive report

## License
MIT

---
For questions or improvements, please open an issue or submit a pull request.
