# Particle System

```javascript
if (!this.particles) {
	log.error('Asset does not contain a ParticleSystem!');
	return;
}
```

The Particles API is available on any Element that contains an Asset with an underlying ParticleSystem.

## Emission

> Emit 10 particles.

```javascript
this.particles.emit(10);
```

> Configure particles to emit when a bool schema value changes.

```javascript
this.particles.emitOn('active', 5);
```

> Cleanup `emitOn` subscription.

```javascript
this.particles.emitOff('active');
```

Particle emission can be driven through the scripting API. 

**Note:** The underlying ParticleSystem's maximum particle count will still control the overall system and may not show all particles created through scripting.

## External Forces

> Enable particles to be influenced by external forces.

```javascript
this.particles.externalForces.enabled = true;
```

> Limit the systems influences by another Element.

```javascript
var field = this.findOne('..(@name=Field)');
this.particles.externalForces.addLimit(field);
```

> Remove an Element as a limit.

```javascript
this.particles.externalForces.removeLimit(field);
```

> Clear all limits from a system's influence list.

```javascript
this.particles.externalForces.clearLimits();
```

External Forces allows emitted particles to be influenced by other particle fields nearby.

**Note:** An Element must contain a Particle System Force Field component from Unity to be added as a limit.
