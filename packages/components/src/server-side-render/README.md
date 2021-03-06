# ServerSideRender

ServerSideRender is a component used for server-side rendering a preview of dynamic blocks to display in the editor. Server-side rendering in a block's `edit` function should be limited to blocks that are heavily dependent on existing PHP rendering logic that is heavily intertwined with data, particularly when there are no endpoints available.

ServerSideRender may also be used when a legacy block is provided as a backward compatibility measure, rather than needing to re-write the deprecated code that the block may depend on.

ServerSideRender should be regarded as a fallback or legacy mechanism, it is not appropriate for developing new features against.

New blocks should be built in conjunction with any necessary REST API endpoints, so that JavaScript can be used for rendering client-side in the `edit` function. This gives the best user experience, instead of relying on using the PHP `render_callback`. The logic necessary for rendering should be included in the endpoint, so that both the client-side JavaScript and server-side PHP logic should require a minimal amount of differences.

## Props

### attributes

An object containing the attributes of the block to be server-side rendered.
E.g: `{ displayAsDropdown: true }`, `{ showHierarchy: true }`, etc...
- Type: `Object`
- Required: No

### block

The identifier of the block to be server-side rendered.
Examples: "core/archives", "core/latest-comments", "core/rss", etc...
- Type: `String`
- Required: Yes

### className

A class added to the DOM element that wraps the server side rendered block.
Examples: "my-custom-server-side-rendered".
- Type: `String`
- Required: No

### urlQueryArgs

Query arguments to apply to the request URL.
E.g: `{ post_id: 12 }`.
- Type: `Object`
- Required: No

## Usage

Render core/archives preview.

```jsx
import { ServerSideRender } from '@wordpress/components';

const MyServerSideRender = () => (
	<ServerSideRender
		block="core/archives"
		attributes={ {
			showPostCounts: true,
			displayAsDropdown: false,
		} }
	/>
);
```

## Output

Output uses the block's `render_callback` function, set when defining the block.

## API Endpoint

The API endpoint for getting the output for ServerSideRender is `/wp/v2/block-renderer/:block`. It will use the block's `render_callback` method.

If you pass `attributes` to `ServerSideRender`, the block must also be registered and have its attributes defined in PHP.

```php
register_block_type(
	'core/archives',
	array(
		'attributes'      => array(
			'showPostCounts'    => array(
				'type'      => 'boolean',
				'default'   => false,
			),
			'displayAsDropdown' => array(
				'type'      => 'boolean',
				'default'   => false,
			),
		),
		'render_callback' => 'render_block_core_archives',
	)
);
```
