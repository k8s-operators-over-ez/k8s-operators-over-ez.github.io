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
- [Ansible Documentation](https://docs.ansible.com/ansible/latest/index.html)

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

![](/assets/resource-controller-reconciliation-cycle-ansible-operators.png)

Each state in the Reconciliation Cycle, correspond to particular points of interest in our Ansible Operator's scaffolding. 

**Resource Controller**: Since our Operator includes an Ansible Role as part of it's scaffolding, the Kubernetes Resource Controller equivalent, is represented by the tasks defined in our Ansible Role. These tasks are generally located at: `roles/operator-name/tasks/main.yml`

**Resource Watching**: Our scaffolded Operator also includes a way to observe resources, through a watch file. This file is located at the root of the scaffolded project, `watches.yaml`. More information on watches [here](https://sdk.operatorframework.io/docs/building-operators/ansible/reference/watches/)

## Observe/Watch

In this phase, the resource controller (Ansible Role) observes the state of the cluster. Typically this is initiated by leveraging a `watches.yaml` file and adding to it resources that should be observed for changes. 

The execution of this stage occurs when we've either created or updated a custom resource. Behind the scenes, the create/update operations are essentially the same operation since all kubernetes resources are idempotent. 

A simplified example definition of a resource might look like the following: 

```yaml
apiVersion: operator.local/v1alpha1
kind: MyCRD
metadata: 
  name: my-app
spec:
  size: 3
```

When this resource is created, part of the resource creation process involves adding "watches" on the resource. A "watch" is essentially an observer, which observes the current state of the resource. For example, they can be added in the `watches.yaml` file as illustrated: 

```yaml
---
# Use the 'create api' subcommand to add watches to this file.
- version: v1alpha1
  group: myoperator.mydomain.com
  kind: MyCRD
  role: mycrd
# +kubebuilder:scaffold:watch
```

## Analyze

In this phase, the resource controller (Ansible Role) compares the current state of the resource instance to the desired state. The desired state is typically reflective of what is specified in the `spec` attributes of the resource. 

An example of a request like this would be if we wanted to change the `spec: size: 3` to `spec: size: 4` of the resource. 

![](/assets/current-state-desired-state.png)

## Act/Reconcile

In this phase, the resource controller (Ansible Role) performs all necessary actions to make the current resource state match the desired state. This is called reconciliation, and is typically where operational knowledge is implemented (i.e. business/domain logic).

Since the current state doesn't match the desired state, the resource controller must reconcile the state differences and make the desired state the new current state. This is done in the `roles/operator-name/tasks/main.yml` file of the Ansible Roles tasks. 

```yaml
# Example Ansible Tasks for an Ansible Role
- name: set up operator reconciliation environment
  set_fact: 
    ansible_operator_meta: 
      namespace: ansible-operator-overeasy-system
  when: ansible_operator_meta.namespace is undefined

- name: call api for response (if message and/or timeout is not defined)
  shell:  'curl -sb -H "Accept: application/json" "http://my-json-server.typicode.com/keunlee/test-rest-repo/golang-lab00-response"'
  register: response_fact
  when: (message is undefined) or (timeout is undefined)

- name: get response
  set_fact: 
    response: '{{response_fact.stdout}}'
  when: (message is undefined) or (timeout is undefined)

- name: set operator specifications on valid response
  set_fact:
    message: '{{response.message}}'
    timeout: '{{response.timeout}}'
  when: response is defined
...
...
...
```