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

.. _board_atmelavr_AT90CAN32:

AT90CAN32
=========

.. contents::

Hardware
--------

Platform :ref:`platform_atmelavr`: Atmel AVR 8- and 32-bit MCUs deliver a unique combination of performance, power efficiency and design flexibility. Optimized to speed time to market-and easily adapt to new ones-they are based on the industrys most code-efficient architecture for C and assembly programming.

.. list-table::

  * - **Microcontroller**
    - AT90CAN32
  * - **Frequency**
    - 16MHz
  * - **Flash**
    - 31KB
  * - **RAM**
    - 2KB
  * - **Vendor**
    - `Microchip <https://www.microchip.com/wwwproducts/AT90CAN32?utm_source=platformio&utm_medium=docs>`__


Configuration
-------------

Please use ``AT90CAN32`` ID for :ref:`projectconf_env_board` option in :ref:`projectconf`:

.. code-block:: ini

  [env:AT90CAN32]
  platform = atmelavr
  board = AT90CAN32

You can override default AT90CAN32 settings per build environment using
``board_***`` option, where ``***`` is a JSON object path from
board manifest `AT90CAN32.json <https://github.com/platformio/platform-atmelavr/blob/master/boards/AT90CAN32.json>`_. For example,
``board_build.mcu``, ``board_build.f_cpu``, etc.

.. code-block:: ini

  [env:AT90CAN32]
  platform = atmelavr
  board = AT90CAN32

  ; change microcontroller
  board_build.mcu = at90can32

  ; change MCU frequency
  board_build.f_cpu = 16000000L

Debugging
---------
:ref:`piodebug` currently does not support AT90CAN32 board.

Frameworks
----------
.. list-table::
    :header-rows:  1

    * - Name
      - Description

    * - :ref:`framework_arduino`
      - Arduino Wiring-based Framework allows writing cross-platform software to control devices attached to a wide range of Arduino boards to create all kinds of creative coding, interactive objects, spaces or physical experiences.