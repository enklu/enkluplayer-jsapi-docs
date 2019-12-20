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

> Cleanup `emitOn` subscription

```javascript
this.particles.emitOff('active');
```

Particle emission can be driven through the scripting API. 

**Note:** The underlying ParticleSystem's maximum particle count will still control the overall system and may not show all particles created through scripting.
