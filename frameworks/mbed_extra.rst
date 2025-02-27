..  Copyright (c) 2014-present PlatformIO <contact@platformio.org>
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at
       http://www.apache.org/licenses/LICENSE-2.0
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

Configuration
-------------

.. contents::
    :local:

Configuration system
~~~~~~~~~~~~~~~~~~~~

PlatformIO allows you to customize mbed OS compile time configuration
parameters using ``mbed_app.json`` manifest. It should be placed into the root
of your project and located on the same level as :ref:`projectconf`.

Configuration is defined using `JSON <https://en.wikipedia.org/wiki/JSON>`_.
Some examples of configuration parameters:

* The sampling period for a data acquisition application.
* The default stack size for a newly created OS thread.
* The receive buffer size of a serial communication library.
* The flash and RAM memory size of a target board.

See more details in the official `ARM Mbed OS Configuration System <https://os.mbed.com/docs/mbed-os/v5.11/reference/configuration.html>`_.

A few PlatformIO-ready projects based on ARM mbed OS which use ``mbed_app.json``;

* `Freescale Kinetis: mbed-rtos-tls-client <https://github.com/platformio/platform-freescalekinetis/tree/develop/examples/mbed-rtos-tls-client>`_
* `ST STM32: mbed-rtos-mesh-minimal <https://github.com/platformio/platform-ststm32/tree/develop/examples/mbed-rtos-mesh-minimal>`_

Mbed lib and Mbed OS 5
~~~~~~~~~~~~~~~~~~~~~~

PlatformIO allows compiling projects with or without Mbed OS. By default, project
is built without the OS feature. Most of the framework functionality requires the OS to be
enabled. To add the OS feature you can use a special macro definition that needs be added to
:ref:`projectconf_build_flags` of :ref:`projectconf`:

.. list-table::
    :header-rows:  1

    * - Name
      - Description

    * - ``PIO_FRAMEWORK_MBED_RTOS_PRESENT``
      - Build the project with enabled ``rtos``

An example of :ref:`projectconf` with enabled ``rtos``

.. code-block:: ini

    [env:wizwiki_w7500p]
    platform = wiznet7500
    framework = mbed
    board = wizwiki_w7500p
    build_flags = -D PIO_FRAMEWORK_MBED_RTOS_PRESENT


Build profiles
~~~~~~~~~~~~~~

By default, PlatformIO builds your project using ``develop profile`` which provides optimized
firmware size with full error information and allows MCU to go to sleep mode. In the case when
default build profile is not suitable for your project there two other profiles ``release`` and
``debug`` that can be enabled using special macro definitions. You can change build profile
:ref:`projectconf_build_flags` of :ref:`projectconf`:

.. list-table::
    :header-rows:  1

    * - Name
      - Description

    * - ``MBED_BUILD_PROFILE_RELEASE``
      - Release profile (smallest firmware, minimal error info)

    * - ``MBED_BUILD_PROFILE_DEBUG``
      - Debug profile (largest firmware, disabled sleep mode)

More information about differences between build profiles can be found on the
official page `ARM Mbed OS Build Profiles <https://os.mbed.com/docs/mbed-os/v5.11/tools/build-profiles.html>`_.

Ignoring particular components
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In case you don't need all parts of the framework or you want to reduce the compilation time, you can explicitly
exclude folders with redundant sources. For example, to remove ``cellular``, ``mbedtls`` and ``nanostack`` features
from the build process, navigate to :ref:`projectconf_pio_packages_dir` and create a new file ``framework-mbed/features/.mbedignore``
with the following contents:

.. code-block:: ini

    cellular/*
    mbedtls/*
    nanostack/*

If you want to exclude the entire folder, simply create ``.mbedignore`` file and add only one symbol ``*`` to this file.

Custom Targets
~~~~~~~~~~~~~~

In case when your board is not officially supported by :ref:`framework_mbed` you can
manually add custom board definitions to your project. First of all, you need to create
a special file ``custom_targets.json`` in the root folder of your project where you
describe your board, for example here is the configuration for ``NUCLEO-F401RE`` board:

.. code-block:: json

    {
      "NUCLEO_F401RE": {
            "inherits": ["FAMILY_STM32"],
            "supported_form_factors": ["ARDUINO", "MORPHO"],
            "core": "Cortex-M4F",
            "extra_labels_add": ["STM32F4", "STM32F401xE", "STM32F401RE"],
            "config": {
                "clock_source": {
                    "help": "Mask value : USE_PLL_HSE_EXTC | USE_PLL_HSE_XTAL (need HW patch) | USE_PLL_HSI",
                    "value": "USE_PLL_HSE_EXTC|USE_PLL_HSI",
                    "macro_name": "CLOCK_SOURCE"
                }
            },
            "detect_code": ["0720"],
            "macros_add": ["USB_STM_HAL", "USBHOST_OTHER"],
            "device_has_add": [
                "SERIAL_ASYNCH",
                "FLASH",
                "MPU"
            ],
            "release_versions": ["2", "5"],
            "device_name": "STM32F401RE"
        }
    }

Secondly, you need to add code specific to your target to the ``src`` folder of your project.
Usually, it's a good idea to isolate this code in a separate folder and add еру path to this folder
to :ref:`projectconf_build_flags` of :ref:`projectconf`:

.. code-block:: ini

  [env:my_custom_board]
  platform = nxplpc
  framework = mbed
  board = my_custom_board
  build_flags = -I$PROJECTSRC_DIR/MY_CUSTOM_BOARD_TARGET

Next, you need to inform PlatformIO that there is a new custom board. To do this, you can create
``boards`` directory in the root folder of your project and add a board manifest file with your
board name, e.g. ``my_custom_board.json`` as described here :ref:`board_creating`

After these steps, your project structure should look like this:

.. code-block:: bash

    project_dir
    ├── include
    ├── boards
    │    └── my_custom_board.json
    ├── src
    │    ├── main.cpp
    │    └── MY_CUSTOM_BOARD_TARGET
    │         ├── pinNames.h
    │         └── pinNames.c
    ├── custom_targets.json
    └── platformio.ini

More information about adding custom targets can be found on the official page
 `Adding and configuring targets <https://os.mbed.com/docs/mbed-os/v5.12/reference/adding-and-configuring-targets.html>`_.

See full examples with a custom board:

- https://github.com/platformio/platform-nxplpc/tree/develop/examples/mbed-custom-target
