.. SPDX-FileCopyrightText: 2022 Zextras <https://www.zextras.com/>
..
.. SPDX-License-Identifier: CC-BY-NC-SA-4.0

.. _videoserver:

|vs|
====

The |vs| is a WebRTC stream aggregator that improves |team|\ 's
performance by merging and decoding/re-encoding all streams in a
Meeting.

While the default WebRTC creates one incoming and one outgoing stream
per meeting participant, with the |vs|, each client will
only have one aggregated inbound stream and one aggregated outbound
stream. This applies for both video and audio.

.. card:: Example

   In our simple scenario, 5 people are participating in a meeting.

   - Without |vs|: each client generates 4x outgoing
     video/audio streams and receives 4 incoming video/audio streams

   - With |vs|: each client generates 1 outgoing video/audio
     stream and receives 1 incoming video/audio stream

   In summary:
   
   +----------------------+------------------------+-----------------------+
   | |vs|                 | Incoming Connections   | Outgoing Connections  |
   +======================+========================+=======================+
   | No                   | 4 (1 *from* each other | 4 (1 *to* each other  |
   |                      | client)                | client)               |
   +----------------------+------------------------+-----------------------+
   | Yes                  | 1                      | 1                     |
   +----------------------+------------------------+-----------------------+

By default, the |vs| uses conservative Codecs (VP8 and Opus) to
ensure the broadest compatibility, but more codecs can be enabled. It
also splits the Webcam and Screen Sharing streams and reserves the same
bandwidth for both.

A properly set up |vs| will supersede the need of a TURN server,
provided that all clients can reach the |vs|’s public IP and
that UDP traffic is not filtered.

.. _vs-installation:

|vs| Installation
-----------------

The installation process of |vs| has been moved as part of the main
installation, please refer to the :ref:`corresponding Step
<vs_installation>`.

.. _vs-architecture:

Architecture and Service Control
--------------------------------

A |team| meeting is hosted **on one mailbox**, which also keeps the state
of the meeting. It is a responsibility of that mailbox to communicate
with a videoserver instance to start a meeting and controlling it.

Therefore, each mailbox has its own connection pool, which can be
controlled via the Carbonio CLI. The commands to control the service
are straightforward:

-  Start the connection pool

   .. code:: console

      zextras$ carbonio chats doStartService chats-video-server-connection-pool

-  Shutdown the connection pool

   .. code:: console

      zextras$ carbonio chats doStopService chats-video-server-connection-pool

-  Check a connection pool status. This command reports information
   about the node *on which it is executed*.

   .. code:: console

      zextras$ carbonio chats clusterstatus

	   isFullySynced                                       true
	   servers
	    <ip_server>
		online                                          true
		min_api_version                                 1
		max_api_version                                 22
	   meeting_servers
	       <ip_videoserver>:8188
		   id                                           123
		   hostname                                     <ip_videoserver>:8188
		   status                                       online
		   servlet_port                                 8090
		   last_failure
		   local_meetings_hosted                        2

   The output of this command contains this information:

   - Should the remote |vs| be offline or unreachable, the
     status will be **offline** instead of **online**.

   - The API versions supported by the server (``min_api_version`` and
     ``min_api_version``)

   - ``last failure`` shows an error message (e.g., *Unauthorized
     request (wrong or missing secret/token)* or a generic *Runtime
     Exception*) if the last connection attempt to the videoserver was
     unsuccessful. The message is cleared when the connection is
     successful.

   - ``local_meetings_hosted`` reports the number of meetings hosted
     on the *current mailbox*.

.. _vs-scaling:

|vs| Scaling
--------------------

Multiple |vs| can be run on the same infrastructure.

To add a new |vs| to the configuration, run the |vs| installer on a
new server and follow the instructions - the installer will provide
the required commands (``carbonio chats video-server add`` with the
appropriate parameters) needed to add the server to the infrastructure
once packages are installed.

To remove a |vs| from the configuration, use the ``carbonio chats
video-server remove`` command from any mailbox server - this will
remove the appropriate entries from the Zextras Config (manual package
removal on the video server is required).

.. once beta is over?
   
.. warning:: When using multiple video servers, meetings are instanced
   on any of the available instances.

.. card:: CLI Commands

    The CLI command to manage |vs| installations is :command`carbonio
    team` with the sub-command ``video-server`` and the parameters
    `add` and `remove`.

   ..
      The CLI command to manage |vs| installations is ``carbonio
      team`` with the parameter ``video-server`` and the parameters
      `video-server add <carbonio_team_video-server_add>` and
      `video-server remove <carbonio_team_video-server_remove>`
      respectively.

   Quick reference:

   .. code:: console

      zextras$ carbonio chats video-server add *videoserver.example.com* [param VALUE[,VALUE]]

      zextras$ carbonio chats video-server remove *videoserver.example.com* [param VALUE[,VALUE]]

.. _vs-bandwidth-and-codecs:

Bandwidth and Codecs
--------------------

.. grid:: 1 1 2 4
   :gutter: 2

   .. grid-item-card:: Video Bandwidth
      :columns: 12 12 6 4

      The administrator can set the webcam stream quality and the screenshare
      stream quality specifing the relative bitrate *in Kbps*. The values must
      be at least 100 Kbps and can be increased as desired.

      Higher values mean more quality but more used bandwidth.

      -  ``carbonio config global set attribute teamChatWebcamBitrateCap value 200``:
         is the command for the webcam stream quality/bandwidth

      -  ``carbonio config global set attribute teamChatScreenBitrateCap value 200``:
         is the command for the screenshare stream qualitybandwidth

      .. tip::

         By default both the webcam bandwidth and the screen sharing bandwidth
         are set to 200 Kbps.

   .. grid-item-card:: Video Codecs
      :columns: 12 12 6 4

      By default, the VP8 video codec is used. This is to ensure the best
      compatibility, as this codec is available in all supported browsers, but
      other codecs can be enabled:

      -  AV1:
         :command:`carbonio config global set attribute teamChatVideoCodecAV1 value true`

      -  H264:
         :command:`carbonio config global set attribute teamChatVideoCodecH264 value true`

      -  H265:
         :command:`carbonio config global set attribute teamChatVideoCodecH265 value true`

      -  VP8:
         :command:`carbonio config global set attribute teamChatVideoCodecVP8 value true`

      -  VP9:
         :command:`carbonio config global set attribute teamChatVideoCodecVP9 value true`

      Only one codec can be enabled at the time, so before enabling a new
      codec remember to disable the previous one using the same command as the
      one in the list above but substituting ``value true`` with
      ``value false``.

      .. container:: informalexample

         E.g. to enable the H264 codec run:

         :command:`carbonio config global set attribute teamChatVideoCodecVP8 value false`

         :command:`carbonio config global set attribute teamChatVideoCodecH264 value true`

   .. grid-item-card:: Audio Codec
      :columns: 12 12 6 4

      The audio codec used by the |vs| is Opus. No other codecs are
      supported, as Opus is currently the only reliable one available across
      all supported browsers.

      .. seealso::

         `Wikipedia page on Opus
         <https://en.wikipedia.org/wiki/Opus_(audio_format)#Bandwidth_and_sampling_rate>`_

.. _vs-advanced-settings:

Advanced Settings
-----------------

The following settings influence the audio experience.

.. grid:: 1 1 2 2
   :gutter: 3

   .. grid-item-card:: Audio Quality
      :columns: 12 12 6 6

      The administrator can set the Opus audio quality by setting the sampling
      rate (in Hz) in the ``teamChatAudioSamplingRate`` global attribute.

      The available values are:

      -  8000 → represents the narrowband bandwidth

      -  12000 → represents the mediumband bandwidth

      -  16000 → represents the wideband bandwidth (**default**)

      -  24000 → represents the superwideband bandwidth

      -  48000 → represents the fullband bandwidth

   .. grid-item-card:: Audio Sensitivity
      :columns: 12 12 6 6

      The administrator can optimize the audio sensitivity with these two
      commands:

      .. code:: console

         zextras$ carbonio config global set attribute teamChatAudioLevelSensitivity value 55

         zextras$ carbonio config global set attribute teamChatAudioSamplingSensitivityInterval value 10

      The audio level sensitivity defines how much the audio should be
      normalized between all the audio sources. The value has a range
      between 0 and 100 where 0 represents the audio muted and 100 the
      maximum audio level (too loud).

      By default the value is set to **55**, which is also the
      value suggested for optimal performances

      The audio sampling sensitivity interval defines the interval in
      seconds used to compute the audio sensitivity level. By default
      the value is set to 2 seconds, this means that the video server
      normalizes the audio level considering the audio sources of the
      last 2 seconds.

      The value should be at least **0**, but it should be set to
      **10** seconds to provide the best performances.
   

.. _vs-record-meeting:

Recording a Video Meeting
-------------------------

The owner or moderator of a room can record any meeting and make it
available for people to watch it later. A meeting can be recorded only
once, meaning that an ongoing recording will be **unique** for that
meeting. In case a recording is interrupted, it can be restarted at a
later point. Every user will be notified of the ongoing recording,
while any moderator in the room can stop it, even if it was started by
another moderator, and save it to a file or to the moderator's |file|.

.. note:: Regardless if the recording is terminated by the person who
   started it or not, a copy of the recording will always be saved in
   the |file| account of who started the recording.

This functionality is provided by a specific package, called
``carbonio-videoserver-recorder``, that **must be installed together**
with ``carbonio-videoserver``. On a Multi-Server, this means that the
package must be installed on each node on which
``carbonio-videoserver`` is installed.

.. note:: All the instructions below must be executed on every node on
   which ``carbonio-videoserver`` is installed, unless differently
   specified.

.. tab-set::

   .. tab-item:: Ubuntu
      :sync: ubuntu
                
      .. code:: console

         # apt install carbonio-videoserver-recorder

   .. tab-item:: RHEL
      :sync: rhel
      
      .. code:: console

         # yum install carbonio-videoserver-recorder

The package installs a service that needs to be associated with the
|vs| instance, a task that needs to be executed from the CLI **on a
Node which installs the** ``carbonio-advanced`` **package** (which is
**SRV3** in our Scenario), using a command that differ depending if
you already installed and configured the |vs| or not.

.. grid:: 1 1 2 2
   :gutter: 3

   .. grid-item-card:: |vs| already installed
      :columns: 12 12 6 6

      If you already installed |vs|, execute this command **on SRV3**:

      .. code:: console

         zextras$ carbonio chats video-server update-servlet example.com:8188 8090

      Here, replace *example.com* with the domain name or IP on which
      the |vs| is installed, *8188* the |vs| port, and *8090* (which
      is the default value) with the servlet port that will be used
      only for recording.

      .. warning:: The value of the servlet port (*8090*) **must**
         match the one defined in file
         :file:`/etc/carbonio/videoserver-recorder/recordingEnv` on **SRV5**.

   .. grid-item-card:: |vs| not yet installed
      :columns: 12 12 6 6

      If you did not yet install |vs|, you can execute the following
      command **on SRV3**, which configures at the same time both the
      |vs| and the recording servlet.

      .. code:: console

         zextras$ carbonio chats video-server add example.com port 8188 servlet_port 8090 secret A_SECRET_PASSWORD

      Replace *example.com* with the actual domain name or IP, *8188*
      and *8090* with the ports associated with the |vs| and the
      recorder, respectively, and *A_SECRET_PASSWORD* with the value
      of the variable ``api_secret`` in file
      :file:`/etc/janus/janus.jcfg` on **SRV5**, for example::
              
              api_secret = "+xpghXktjPGGRIs7Y7ryoeBvW9ReS8RQ"

   .. grid-item-card:: In both cases
      :columns: 12

      In both cases, edit the file :file:`/etc/janus/janus.jcfg`, find
      the variable ``nat_1_1_mapping`` and write the **public IP
      address** of |carbonio| as the value for that variable, for
      example: ``nat_1_1_mapping = "93.184.216.34"``.

Configure |vs| Recording
~~~~~~~~~~~~~~~~~~~~~~~~

To complete the setup, you need to execute a few commands as the
``zextras`` user on **SRV3**. First, make sure that the functionality
is enabled on the infrastructure at COS level.

.. code:: console

   zextras$ carbonio config set cos attribute teamChatEnabled value true

You need then to enable the actual recording on the rooms.

.. code:: console
          
   zextras$ carbonio config set global teamVideoServerRecordingEnabled true

Finally, allow all users to start a recording.
   
.. code:: console
          
   zextras$ carbonio config set global teamMeetingRecordingEnabled true

.. note:: In this command, the policy allows every user to record a
   meeting. It is however possible to enforce this policy at user or
   COS level, to allow only selected users or members of a COS to
   record meetings.

Modify or Move a |vs| Installation
----------------------------------

To reconfigure an existing |vs| instance, simply use the various
commands previously mentioned in this Section, then restart the
|vs| service.

If you prefer to move the |vs| to a different Node, or need to do so
because for example the current Node must be decommissioned, you first
need to remove the |vs| instance: as the ``zextras`` user run

.. code:: console

   zextras$ carbonio chats videoserver remove video.example.com 

Here, *video.example.com* is the name of the |vs| instance, that you
can retrieve as the ``hostname`` in the output of

.. code:: console

   zextras$ carbonio chats clusterstatus 

Once done, remove the package

.. tab-set::

   .. tab-item:: Ubuntu
      :sync: ubuntu

      .. code:: console

         # apt remove service-discover-agent carbonio-videoserver

   .. tab-item:: RHEL
      :sync: rhel

      .. code:: console

         # dnf remove service-discover-agent carbonio-videoserver

Now the |vs| is completely removed from the node and you can install
it on a different Node, using the :ref:`installation procedure <srv5-install>`.
