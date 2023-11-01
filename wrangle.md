### Anti-Aliased Noise ####

```
float vop_fbmNoiseFF(float pos; float rough; int maxoctaves; string noisetype)
float vop_fbmNoiseFV(vector pos; float rough; int maxoctaves; string noisetype)
float vop_fbmNoiseFP(vector4 pos; float rough; int maxoctaves; string noisetype)
vector vop_fbmNoiseVF(float pos; float rough; int maxoctaves; string noisetype)
vector vop_fbmNoiseVV(vector pos; float rough; int maxoctaves; string noisetype)
vector vop_fbmNoiseVP(vector4 pos; float rough; int maxoctaves; string noisetype)
```

```
#include <voplib.h>
vector4 pos = set(v@P.x, v@P.y, v@P.z, 0);
vector4 freq = set(ch("freq"), ch("freq"), ch("freq"), 0);
vector4 phase = set(0, @Time*ch("phase"), 0, 0);
vector noise = vop_fbmNoiseFP(pos*freq + phase, ch("rough"), chi("octaves"), 'noise');
```

### Vein Noise ###
```
vector npos = v@P/1. + set(0., 666., 0.);   // Noise input 3D position
float namp = 1.;                            // namp (Noise amplitude)
float nval = 0., nweight = 0.;              // Init nval (Noise output value), and nweight (Used to normalize octaves)
int oct = 9;                                // Number of Octaves
for( int i = 0; i < oct; i++ )    {
    float __nval = fit(abs(-0.5+noise(set(npos.x,npos.y,npos.z,f@Time))), 0.0, 0.1, 1., 0.);
    nval += __nval * namp;                  // Amplitude
    nweight += namp;                        // Accumulate weight
    npos *= 2.132433;                       // Lacunarity
    namp *= 0.666;                          // Roughness
}
v@Cd = 1 - pow(nval / nweight, 0.8765);     // Visualize Noise Output
```