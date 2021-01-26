Disassemble
===========

.. versionadded:: 0.16

.. module:: ezdxf.disassemble

This module provide tools for the recursive decomposition of DXF entities into
a flat stream of DXF entities and converting DXF entities into generic
:class:`~ezdxf.render.path.Path` and :class:`~ezdxf.render.mesh.MeshBuilder`
objects.

The :class:`~ezdxf.entities.Hatch` entity is special because this entity can
not be reduced into as single path or mesh object. The :func:`make_primitive`
function returns an empty primitive, instead use the :func:`to_primitives`
function to convert a :class:`~ezdxf.entities.Hatch` entity in multiple
(boundary) path objects::

    paths = list(to_primitives([hatch_entity]))


.. warning::

    Do not expect advanced vectorization capabilities: Text entities like TEXT,
    ATTRIB, ATTDEF and MTEXT get only a rough bounding box representation.
    VIEWPORT and IMAGE are represented by their clipping path. Unsupported
    entities: all ACIS based entities, WIPEOUT, XREF, UNDERLAY, ACAD_TABLE, RAY,
    XLINE. Unsupported entities will be ignored.

.. autofunction:: recursive_decompose(entities: Iterable[DXFEntity]) -> Iterable[DXFEntity]

.. autofunction:: to_primitives(entities: Iterable[DXFEntity]) -> Iterable[AbstractPrimitive]

.. autofunction:: to_vertices(primitives: Iterable[AbstractPrimitive]) -> Iterable[Vec3]

.. autofunction:: make_primitive(entity: DXFEntity, max_flattening_distance=None) -> AbstractPrimitive

.. class:: AbstractPrimitive

    Interface class for path/mesh primitives.

    .. attribute:: entity

    Reference to the source DXF entity of this primitive.

    .. attribute:: max_flattening_distance

    The `max_flattening_distance` attribute defines the max distance in drawing
    units between the approximation line and the original curve.
    Set the value by direct attribute access. (float) default = 0.01

    .. autoproperty:: path

    .. autoproperty:: mesh

    .. autoproperty:: is_empty

    .. automethod:: vertices() -> Iterable[Vec3]

