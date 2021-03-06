.. automodule:: boogie.fields

======
Fields
======

Django Boogie defines a few fields for your convenience.


EnumField
=========

Django support for enumerations is based on the choices argument of integer or
text-based fields. Now that Python 3 supports Enum types, this approach is
sub-optimal and also involves lots of undesirable boilerplate.


.. ignore-next-block
.. code-block:: python

    from django.db import models

    class User(models.Model):
        ROLE_TEACHER = 0
        ROLE_STUDENT = 1
        ROLE_CHOICES = [
            (ROLE_TEACHER, 'teacher'),
            (ROLE_STUDENT, 'student'),
        ]
        name = models.CharField(max_length=140)
        role = models.IntegerField(choices=ROLE_CHOICES)

        def can_create_classrooms(self):
            """
            Only teachers can create classrooms.
            """
            return self.role == self.ROLE_TEACHER


Now we can define a RoleEnum and use a EnumField in order to simplify things.

.. ignore-next-block
.. code-block:: python

    from boogie import models
    from boogie.fields import DescriptionEnum


    class RoleEnum(DescriptionEnum):
        TEACHER = 0, 'teacher'
        STUDENT = 1, 'student'

    class User(models.Model):
        name = models.CharField(max_length=140)
        role = models.EnumField(RoleEnum)

        def can_create_classrooms(self):
            """
            Only teachers can create classrooms.
            """
            return self.role == self.ROLE_TEACHER


The EnumField accepts standard Python :class:`Enum` and :class:`IntEnum`
classes. Boogie defines the corresponding :class:`Enum` :class:`IntEnum` that
provides human-friendly names for each enumeration and thus integrates
more nicely with Django and gettext.

A model that declares a :class:`EnumField` is automatically filled with all
possible ROLE_* attributes for each value in the enumeration. :class:`EnumField`s
automatically computes the 'choices' argument, and users cannot override it.


API Documentation
=================

.. autoclass:: EnumField
    :members:

.. autoclass:: Enum
    :members:

.. autoclass:: IntEnum
    :members:

