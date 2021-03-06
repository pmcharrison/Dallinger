Configuration
=============

The Dallinger ``configuration`` module provides tools for reading and writing
configuration parameters that control the behavior of an experiment. To use the
configuration, first import the module and get the configuration object:

::

    import dallinger

    config = dallinger.config.get_config()

You can then get and set parameters:

::

    config.get("duration")
    config.set("duration", 0.50)

When retrieving a configuration parameter, Dallinger will look for the parameter
first among environment variables, then in a ``config.txt`` in the experiment
directory, and then in the ``.dallingerconfig`` file, using whichever value
is found first. If the parameter is not found, Dallinger will use the default.

Built-in configuration
----------------------

Built-in configuration parameters, grouped into categories:

General
~~~~~~~

``mode`` *unicode*
    Run the experiment in this mode. Options include ``debug`` (local testing),
    ``sandbox`` (MTurk sandbox), and ``live`` (MTurk).

``logfile`` *unicode*
    Where to write logs.

``loglevel`` *unicode*
    A number between 0 and 4 that controls the verbosity of logs, from ``debug``
    to ``critical``. Note that ``dallinger debug`` ignores this setting and always
    runs at 0 (``debug``).


``whimsical`` *boolean*
    What's life without whimsy? Controls whether email notifications sent
    regarding various experiment errors are whimsical in tone, or more
    matter-of-fact.


Recruitment (General)
~~~~~~~~~~~~~~~~~~~~~

``auto_recruit`` *boolean*
    A boolean on whether recruitment should be automatic.

``browser_exclude_rule`` *unicode - comma separated*
    A set of rules you can apply to prevent participants with unsupported web
    browsers from participating in your experiment.

``recruiter`` *unicode*
    The recruiter class to use during the experiment run. While this can be a
    full class name, it is more common to use the class's ``nickname`` property
    for this value; for example ``mturk``, ``cli``, ``bots``, or ``multi``.
    NOTE: when running in debug mode, the HotAir (``hotair``) recruiter will
    always be used. The exception is if the ``--bots`` option is passed to
    ``dallinger debug``, in which case the BotRecruiter will be used instead.

``recruiters`` *unicode - custom format*
    When using multiple recruiters in a single experiment run via the ``multi``
    setting for the ``recruiter`` config key, ``recruiters`` allows you to
    specify which recruiters you'd like to use, and how many participants to
    recruit from each. The special syntax for this value is:

    ``recruiters = [nickname 1]: [recruits], [nickname 2]: [recruits], etc.``

    For example, to recruit 5 human participants via MTurk, and 5 bot participants,
    the configuration would be:

    ``recruiters = mturk: 5, bots: 5``


Amazon Mechanical Turk Recruitment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``aws_access_key_id`` *unicode*
    AWS access key ID.

``aws_secret_access_key`` *unicode*
    AWS access key secret.

``aws_region`` *unicode*
    AWS region to use. Defaults to ``us-east-1``.

``ad_group`` *unicode*
    Obsolete. See ``group_name``.

``assign_qualifications`` *boolean*
    A boolean which controls whether an experiment-specific qualification
    (based on the experiment ID), and a group qualification (based on the value
    of ``group_name``) will be assigned to participants by the recruiter.
    This feature assumes a recruiter which supports qualifications,
    like the ``MTurkRecruiter``.

``group_name`` *unicode*
    Assign a named qualification to workers who complete a HIT.

``qualification_blacklist`` *unicode - comma seperated*
    Comma-separated list of qualification names. Workers with qualifications in
    this list will be prevented from viewing and accepting the HIT.

``title`` *unicode*
    The title of the HIT on Amazon Mechanical Turk.

``description`` *unicode*
    The description of the HIT on Amazon Mechanical Turk.

``keywords`` *unicode*
    A comma-separated list of keywords to use on Amazon Mechanical Turk.

``lifetime`` *integer*
    How long in hours that your HIT remains visible to workers.

``duration`` *float*
    How long in hours participants have until the HIT will time out.

``us_only`` *boolean*
    Controls whether this HIT is available only to MTurk workers in the U.S.

``base_payment`` *float*
    Base payment in U.S. dollars. All workers who accept the HIT are guaranteed
    this much compensation.

``approve_requirement`` *integer*
    The percentage of past MTurk HITs that must have been approved for a worker
    to qualify to participate in your experiment. 1-100.

``organization_name`` *unicode*
    Obsolete.


Preventing Repeat Participants
""""""""""""""""""""""""""""""

If you set a ``group_name`` and ``assign_qualifications`` is also set to
``true``, workers who complete your HIT will be given an MTurk qualification for
your ``group_name``. In the future, you can prevent these workers from
participating in a HIT with the same ``group_name`` by including that name in
the ``qualification_blacklist`` configuration. These four configuration keys
work together to create a system to prevent recuiting workers who have already
completed a prior run of the same experiment.


Email Notifications
~~~~~~~~~~~~~~~~~~~

See :doc:`Email Notification Setup <email_setup>` for a much more detailed
explanation of these values and their use.

``contact_email_on_error`` *unicode*
    The email address used as the recipient for error report emails, and the email displayed to workers when there is an error.

``dallinger_email_address`` *unicode*
    An email address for use by Dallinger to send status emails.

``smtp_host`` *unicode*
    Hostname and port of a mail server for outgoing mail. Defaults to ``smtp.gmail.com:587``

``smtp_username`` *unicode*
    Username for outgoing mail host.

``smtp_password`` *unicode*
    Password for the outgoing mail host.


Deployment Configuration
~~~~~~~~~~~~~~~~~~~~~~~~

``database_url`` *unicode*
    URI of the Postgres database.

``database_size`` *unicode*
    Size of the database on Heroku. See `Heroku Postgres plans <https://devcenter.heroku.com/articles/heroku-postgres-plans>`__.

``dyno_type`` *unicode*
    Heroku dyno type to use. See `Heroku dynos types <https://devcenter.heroku.com/articles/dyno-types>`__.

``redis_size`` *unicode*
    Size of the redis server on Heroku. See `Heroku Redis <https://elements.heroku.com/addons/heroku-redis>`__.

``num_dynos_web`` *integer*
    Number of Heroku dynos to use for processing incoming HTTP requests. It is
    recommended that you use at least two.

``num_dynos_worker`` *integer*
    Number of Heroku dynos to use for performing other computations.

``host`` *unicode*
    IP address of the host.

``port`` *unicode*
    Port of the host.

``clock_on`` *boolean*
    If the clock process is on, it will perform a series of checks that ensure
    the integrity of the database.

``heroku_python_version`` *unicode*
    The python version to be used on Heroku deployments. The version specification will
    be deployed to Heroku in a `runtime.txt` file in accordance with Heroku's deployment
    API. Note that only the version number should be provided (eg: "2.7.14") and not the
    "python-" prefix included in the final `runtime.txt` format.
    See Dallinger's `global_config_defaults.txt` for the current default version.
    See `Heroku supported runtimes <https://devcenter.heroku.com/articles/python-support#supported-runtimes>`__.

``heroku_team`` *unicode*
    The name of the Heroku team to which all applications will be assigned.
    This is useful for centralized billing. Note, however, that it will prevent
    you from using free-tier dynos.

``worker_multiplier`` *float*
    Multiplier used to determine the number of gunicorn web worker processes
    started per Heroku CPU count. Reduce this if you see Heroku warnings
    about memory limits for your experiment. Default is `1.5`


Choosing configuration values
-----------------------------

When running real experiments it is important to pick configuration variables that
result in a deployment that performs appropriately.

The number of Heroku dynos that are required and their specifications can make a
very large difference to how the application behaves.

``num_dynos_web``
    This configuration variable determines how many dynos are run to deal with
    web traffic. They will be transparently load-balanced, so the more web dynos are
    started the more simultaneous HTTP requests the stack can handle.
    If an experiment defines the ``channel`` variable to subscribe to websocket events
    then all of these callbacks happen on the dyno that handles the initial ``/launch``
    POST, so experiments that use this functionality heavily receive significantly
    less benefit from increasing ``num_dynos_web``.
    The optimum value differs between experiments, but a good rule of thumb is 1 web
    dyno for every 10-20 simultaneous human users.

``num_dynos_worker``
    Workers are dynos that pull tasks from a queue and execute them in the background.
    They are optimized for many short tasks, but they are also used to run bots which
    are very long-lived. Each worker can run up to 20 concurrent tasks, however they
    are co-operatively multitasked so a poorly behaving task can cause all others
    sharing its host to block.
    When running with bots, you should always pick a value of ``num_dynos_worker` that
    is at least ``0.05*number_of_bots``, otherwise it is guaranteed to fail. In practice,
    there may well be experiment-specific tasks that also need to execute, and bots are
    more performant on underloaded dynos, so a better heuristic is ``0.25*number_of_bots``.

``dyno_type``
    This determines how powerful the heroku dyno that's started is. It applies to both
    web and worker dyno types. The minimum recommended is ``standard-1x``, which should be
    sufficient for experiments that do not rely on real-time coordination, such as
    :doc:`demos/bartlett1932/index`.
    Experiments that require significant power to process websocket events should consider
    the higher levels, ``standard-2x``, ``performance-m`` and ``performance-l``. In all but
    the most intensive experiments, either ``dyno_type`` or ``num_dynos_web`` should be
    increased, not both.

``redis_size``
    A larger value for this increases the number of connections available on the redis dyno.
    This should be increased for experiments that make substantial use of websockets. Values
    are ``premium-0`` to ``premium-14``. It is very unlikely that values higher than ``premium-5``
    are useful.

``duration``
    The duration parameter determines the number of hours that an MTurk worker has to complete
    the experiment. Choosing numbers that are too short can cause people to refuse to work on
    a HIT. A deadline that is too long may give people pause for thought as it may make
    the task seem underpaid. Set this to be significantly above the total time from start
    to finish that you'd expect a user to take in the worst case.

``base_payment``
    The amount of US dollars to pay for completion of the experiment. The higher this is,
    the easier it will be to attract workers.
