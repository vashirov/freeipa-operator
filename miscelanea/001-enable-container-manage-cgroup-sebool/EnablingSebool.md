# Enabling sebool

To enable the container_manage_cgroup sebool has been tried 3 different
methods:

- **Direct change by using setsebool** in the worker nodes one by one: This
  change the sebool but it only makes sense in a development environement.
- **Using Tuned Operator by a Tuned object**: This is the wrong path to make
  this changes, as we can see at:
  - [Is this operator capable of setting sebools?](https://github.com/openshift/cluster-node-tuning-operator/issues/89).
  - [Manage sebooleans in MachineConfig](https://github.com/openshift/machine-config-operator/issues/852).
  However, the operator provide interesting features to keep in mind for the
  future. Look at [tune-sebool.yaml](tune-sebool.yaml) file for more
  information.
- **Using Machine Config Operator by a MachineConfig object**: This has been
  the final path which has worked for us. It provides a way to change
  the sebool using the Kubernetes way.

To enable the container_manage_cgroup sebool, just run as administrator:

```shell
oc create -f 10-enable-container-manage-cgroup-sebool.yaml
```

To disable the container_manage_cgroup sebool, just run as administrator:

```shell
oc delete -f 10-enable-container-manage-cgroup-sebool.yaml
```

## Contents

- [10-enable-container-manage-cgroup-sebool.yaml](10-enable-container-manage-cgroup-sebool.yaml)
  contains the MachineConfig object that works.
- [10-wrong-enable-container-manage-cgroup-sebool.yaml](10-wrong-enable-container-manage-cgroup-sebool.yaml)
  a wrong version from the above, which replay the situation where no
  traces or validation error are got when applying the object making
  difficult to detect what is wrong in the object definition. Using this
  to provide some input to the MCO team if it could be enhanced to print out
  some log traces about it.
- [tune-sebool.yaml](tune-sebool.yaml) file is an example trying to use
  Tune Operator, which does not support managing sebool.
