## `SDL_CreateRGBSurface`

### Documentation

Allocate a new *RGB surface*.

#### Syntax

```c
SDL_Surface* SDL_CreateRGBSurface(
  Uint32 flags,
  int width,
  int height,
  int depth,
  Uint32 Rmask,
  Uint32 Gmask,
  Uint32 Bmask,
  Uint32 Amask
);
```

#### Function Parameters

1. `flags`: the flags are *unused* and should be set to `0`
2. `width`: the width of the surface
3. `height`: the height of the surface
4. `depth`: the depth of the surface in bits
5. `Rmask`: the red mask for the pixels
6. `Gmask`: the green mask for the pixels
7. `Bmask`: the blue mask for the pixels
8. `Amask`: the alpha mask for the pixels

#### Return Value

Returns a new `SDL_Surface` structure that is created or `NULL` if it fails; call `SDL_GetError()` for more information.

#### Remarks

1. If `depth` is 4 or 8 bits, an empty palette is allocated for the surface. If `depth` is greater than 8 bits, the pixel format is set using the [RGBA] mask parameters.

2. The [RGBA] mask parameters are the *bitmasks* used to extract the color from a pixel. For instance, `Rmask` being `0xFF000000` means the *red data* is stored in the *most significant byte*. Using zeros for the RGB masks sets a default value, based on the depth. For example:

```c
SDL_CreateRGBSurface(0, w, h, 32, 0, 0, 0, 0);
```

3. However, using zero for the Amask results in an Amask of 0.

4. By default surfaces with an alpha mask are set up for blending as with:

```c
SDL_SetSurfaceBlendMode(surface, SDL_BLENDMODE_BLEND);
```

5. You can change this by calling `SDL_SetSurfaceBlendMode()` and selecting a different `blendeMode`.

#### Version

This function is available since SDL 2.0.0.

#### Code Examples

```c
/* Create a 32-bit surface with the bytes of each pixel in R, G, B, A order, as expected by OpenGL for textures */
SDL_Surface *surface;
Uint32 rmask, gmask, bmask, amask;

/* SDL interprets each pixel as a 32-bit number, so our masks must depend on the endianness (byte order) of the machine */
#if SDL_BYTEORDER == SDL_BIG_ENDIAN
  rmask = 0xff000000;
  gmask = 0x00ff0000;
  bmask = 0x0000ff00;
  amask = 0x000000ff;
#else
  rmask = 0x000000ff;
  gmask = 0x0000ff00;
  bmask = 0x00ff0000;
  amask = 0xff000000;
#endif

surface = SDL_CreateRGBSurface(
  0, width, height, 32,
  rmask, gmask, bmask, amask
);

if (surface == NULL) {
  SDL_Log("SDL_CreateRGBSurface() failed: %s", SDL_GetError());
  exit(1);
}

/* or using the default masks for the depth: */
surface = SDL_CreateRGBSurface(0, width, height, 32, 0, 0, 0, 0);
```

#### Related Functions

* `SDL_CreateRGBSurfaceFrom`
* `SDL_CreateRGBSurfaceWithFormat`
* `SDL_FreeSurface`
