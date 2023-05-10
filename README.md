# Homa Performance

Comparing the performance of [Homa](https://networking.harshkapadia.me/homa) with [TCP](https://networking.harshkapadia.me/tcp).

## CloudLab Setup

-   Set up an account on [CloudLab](https://cloudlab.us), as that is the testbed on which the experiments rely heavily on.
    -   Going through [CloudLab's basic concepts](https://docs.cloudlab.us/basic-concepts.html) in its documentation is highly recommended.
    -   Following [CloudLab's OpenStack tutorial](https://docs.cloudlab.us/openstack-tutorial.html) in its documentation is a helpful way to get used to CloudLab.
-   Instantiate the [`homa-test-2` profile](https://www.cloudlab.us/p/CloudLab/homa-test-2).

    -   This profile runs on the [`xl170` machines](https://www.utah.cloudlab.us/portal/show-nodetype.php?type=xl170) that CloudLab has, so please [check their availability](https://www.cloudlab.us/resinfo.php) and [extend the experiment date](https://docs.cloudlab.us/basic-concepts.html#%28part._extending%29).
    -   Bandwidth set in the profile: 25 Giga Bts per Second (i.e., 10 Gbps, not 10 GBps).
    -   Image set in the profile: Ubuntu 22.04.2 LTS (`jammy`)

        -   This can be checked using the command

            ```shell
            lsb_release -a
            ```

-   Once the experiment status is `ready`, [SSH](https://networking.harshkapadia.me/ssh) into both the nodes.

    Example:

    ```shell
    ssh harshk0@hp000.utah.cloudlab.us -i ./.ssh/private_key_file
    ```

-   Homa has to be operated on specific Linux kernel versions. These experiments were run on `Linux 5.18.0-051800-generic x86_64`, essentially version 5.18.0 of the Linux kernel.

    -   Check the Linux kernel version

        ```shell
        uname -srm
        ```

    -   Set the kernel to 5.18.0 if required

        -   [Install the `add-apt-repository` command if required](https://phoenixnap.com/kb/add-apt-repository-command-not-found-ubuntu)
        -   Follow this: [How To Install a Mainline Linux Kernel in Ubuntu](https://stevescargall.com/blog/2023/04/21/how-to-install-a-mainline-linux-kernel-in-ubuntu)
        -   Update all the package lists

            ```shell
            sudo apt-get update
            ```

## Build Homa Module

-   Clone the Homa module in the home (`~`) directory.
-   Change directories into the cloned `HomaModule` directory.
-   Run `make`.
-   Change directories to the `util` directory and run `make`.
-   Clone this repository, cd into the cloned `homa-perf` directory and run the `copy-homa-files.sh` script.

    ```shell
    cd ~/homa-perf
    sudo chmod +x copy-homa-files.sh
    ./copy-homa-files.sh
    ```

## Configure Homa Module

-   Replace the `~/bin/config` script with the script in this repository.

    ```shell
    cd ~/homa-perf
    mv ./config ~/bin/
    ```

-   Change directories to `~/bin` and run the newly replaced `config` script with the `default` argument.

    ```shell
    cd ~/bin
    chmod +x config
    config default
    ```

-   Repeat these steps in both `node0` and `node1` on CloudLab.

## Install Homa module

-   Run the `~/HomaModule/cloudlab/bin/install` script.

    ```shell
    cd ~/HomaModule/cloudlab/bin
    chmod +x install
    install
    ```

## Running the Experiment

-   On CloudLab's `node1`, run `cp_node server`.
-   On CloudLab's `node0`, run `cp_node client`.
-   [More experiments and details](https://github.com/PlatformLab/HomaModule/tree/master/util)
