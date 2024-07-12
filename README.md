# boot2root

boot2root is a cybersecurity challenge developed as part of the 42 École curriculum. The objective of the project is to gain root access to a virtual machine by exploiting vulnerabilities.

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Objectives](#objectives)
- [Approach](#approach)
- [Tools and Techniques](#tools-and-techniques)
- [Contact](#contact)

## Introduction

The boot2root project is designed to teach students about various aspects of computer security, including network protocols, system vulnerabilities, and penetration testing techniques. The goal is to identify and exploit vulnerabilities in a virtual machine to gain root access.

## Features

- Real-world security challenges.
- Hands-on experience with penetration testing.
- Understanding of system and network vulnerabilities.
- Learning and applying various cybersecurity tools and techniques.

## Installation

To set up the boot2root environment, follow these steps:

1. **Clone the repository**
   ```
   sh
   git clone https://github.com/your_username/boot2root.git
   cd boot2root
   ```
    Set up the virtual machine
        Download the provided virtual machine image.
        Import the virtual machine into your preferred virtualization software (e.g., VirtualBox, VMware).

    Start the virtual machine
        Configure the network settings as required (usually NAT or Bridged Adapter).
        Start the virtual machine.

## Usage

The objective is to gain root access to the virtual machine. Use various tools and techniques to identify and exploit vulnerabilities. Document your process and findings.
## Objectives

    Identify vulnerabilities in the virtual machine.
    Exploit vulnerabilities to gain access.
    Escalate privileges to gain root access.
    Document the entire process, including tools used and steps taken.

## Approach

    Information Gathering: Collect information about the target system using tools like nmap, netcat, and wireshark.
    Vulnerability Analysis: Identify potential vulnerabilities in services, applications, and configurations.
    Exploitation: Use exploit tools and techniques to gain access to the system.
    Privilege Escalation: Escalate privileges to gain root access.
    Documentation: Document all findings, steps taken, and tools used.

## Tools and Techniques

    Network Scanning: nmap, netdiscover
    Exploitation: exploit-db (dirty_cow)
    Privilege Escalation: sudo

## Contact

    Erik Grigoryan - ergrigor@student.42yerevan.am
