# Particle System

```javascript
if (!this.particles) {
	log.error('Asset does not contain a ParticleSystem!');
	return;
}
```

The Particles API is available on any Element that contains an Asset with an underlying ParticleSystem.

## Emission

```javascript
// Emit 10 particles.
this.particles.emit(10);

// Configure particles to emit when a bool schema value changes.
this.particles.emitOn('active', 5);

// Cleanup `emitOn` subscription
this.particles.emitOff('active');
```

Particle emission can be driven through the scripting API. 

<b>Note:</b> The underlying ParticleSystem's maximum particle count will still control the overall system and may not show all particles created through scripting.
