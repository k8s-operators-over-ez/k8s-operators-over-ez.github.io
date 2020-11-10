---
title: Ansible Operators
---

## Prerequisites

- Review the [introduction](../introduction), if you have not already
- Familiarity with Ansible.

## Agenda

This section will cover the following: 

- Review options for developing with Operators with Ansible
- Review the [Reconciliation Cycle](../01/01-introduction.md#how-do-operators-work) with Ansible semantics

Afterwards, you will take the plunge in a guided walkthrough. 

# Ansible Operator Resources

## Development Libraries

Checkout out the following resource for developing Operators with Ansible: 

- [Operator Framework](https://operatorframework.io/)

{{<hint info>}}
In this section, as we discuss Ansible Operators, we will be referring to the __Operator Framework__, as we continue to discuss. 
{{</hint>}}

# The Reconciliation Cycle - Revisited

{{<hint info>}}
In the introduction, we presented the reconciliation cycle in a resource controller as followed: 
{{</hint>}}

![](/assets/resource-controller-reconciliation-cycle.png)  

{{<hint info>}}
In this section, we will recap the Reconciliation Cycle in more detail with Ansible Operator specific embellishments to the Reconciliation Cycle, as shown below:  
{{</hint>}}