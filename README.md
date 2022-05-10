# bpmn-js CustomElements

## customRendererModule



```js
import BaseRenderer from 'diagram-js/lib/draw/BaseRenderer';
import {
  getRoundRectPath
} from 'bpmn-js/lib/draw/BpmnRenderUtil';
import {
  is,
  getBusinessObject
} from 'bpmn-js/lib/util/ModelUtil';

/**
* 判断图形的类型是否符合指定
* @is ( shape: bpmnShape , type: string )=> boolean
*
* @getBusinessObject ( element )=> bpmnBusinessObject
*
* @bpmnBusinessObject {
*   suitable:number
* }
*/

const HIGH_PRIORITY = 1500,
      TASK_BORDER_RADIUS = 2

export default class CustomRenderer extends BaseRenderer {
    constructor( eventBus , bpmnRenderer ){
        super( eventBus , HIGH_PRIORITY )
        /**
        * @bpmnRenderer ( shape )=> ShapePath
        */
        this.bpmnRenderer = bpmnRenderer
    }
    
    canRender(element){
        return Boolean
    }
    drawShape( parentNode , element ){
       const shape = this .bpmnRenderer .drawShape(parentNode, element);
        
        return shape
    }
    getShapePath(shape){
        /**
        *@getRoundRectPath ( shape , TASK_BORDER_RADIUS )=> shapePath
        *@bpmnRenderer
            .getShapePath
            ( shape )=> shapePath
        */
        this.bpmnRenderer
            .getShapePath
        return getRoundRectPath( shape , TASK_BORDER_RADIUS )
    }
} 
CustomRenderer.$injects = [
    "eventBus","bpmnRenderer"
]
```





## customPaletteModule



```js
export default class CustomPalette {
    constructor( bpmnFactory, create, elementFactory, palette, translate ){
    	/*
    	bpmnFactory {
    		create:( type : string )=> businessObject
    	}
    	*/
    	this.bpmnFactory = bpmnFactory;
    	/*
    	create {
    	start : ( event , shape )=> void
    	}
    	*/
    	this.create = create;
    	/*
    	elementFactory {
    	createShape: ( shapeOptions : {
    		type: string
    		businessObject: businessObject
    	} )=> shape
    	}
    	*/
    	this.elementFactory = elementFactory;
    	/*
    	translate {
    	( text: string ): string
    	}
    	*/
    	this.translate = translate;
    	/*
    	palette {
    		registerProvider : ( module )=>void
    	}
    	*/
    	palette .registerProvider ( this );
    }
    getPaletteEntries(element){
        /* return PaletteEntries : {
        	[ entryName : string ]: Entry {
        		group:string
        		className:string
        		title:string
        		action:{
        			dragstart: ( event )=> void
        			click: ( event )=> void
        		}
        	}
        } 
        */
    	const {
      		bpmnFactory,
      		create,
      		elementFactory,
      		translate
    	} = this;
        function createTask ( ...props ) {
            return function ( event ) {
                const businessObject = bpmnFactory .create( 'bpmn:Task' )
                businessObejct[ propName ] = propValue;
                const shape = elementFactory .createShape( {
                    type : 'bpmn:Task',
                    businessObject,
                } )
                create .start( event, shape )
            }
        }
        return {
            'create.elementype':{
        		group: 'activity',
                className: 'bpmn-icon-task red',
                title: '',
        		action: {
          			dragstart: createTask( ...prop[] ),
          			click: createTask( ...prop[] )
        		}
            },
            ...
        }
    }
}
CustomPalette.$injects = [
  'bpmnFactory',
  'create',
  'elementFactory',
  'palette',
  'translate'
]
```





## costomContextPadModule



```js
export default class CustomContextPad {
  constructor( bpmnFactory, config, contextPad, create, elementFactory, injector, translate ) {
    this.bpmnFactory = bpmnFactory;
    this.create = create;
    this.elementFactory = elementFactory;
    this.translate = translate;
    if ( config .autoPlace !== false ) {
      this .autoPlace = injector .get( 'autoPlace', false );
    }
    contextPad .registerProvider( this );
  }
    getContextPadEntries( element ) {
        const {
          autoPlace,
          bpmnFactory,
          create,
          elementFactory,
          translate
        } = this;
        function appendByClick (...props) {
            return function handleAppendByClick ( event , element ){
                if (autoPlace) {
                  const businessObject = bpmnFactory.create('bpmn:Task');

                  businessObject[propName] = propValue;

                  const shape = elementFactory.createShape({
                    type: 'bpmn:Task',
                    businessObject: businessObject
                  });

                  autoPlace.append(element, shape);
                } else {
                  appendServiceTaskStart(event, element);
                }
            }
        }
        function appendByDrag (...props) {
            return function handleAppendByDrag ( event ){
        		const businessObject = bpmnFactory .create('bpmn:Task');

        		businessObject .suitable = suitabilityScore;

        		const shape = elementFactory .createShape( {
          			type: 'bpmn:Task',
          			businessObject: businessObject
        		} );

        		create .start(event, shape, element);
            }
        }
        /*
        @ContextPadEntries {
            ['append.elementType']: {
                group
                className
                title
                action:{
                    click: handleAppendByClick (event)=>void
                    dragstart: handleAppendByDrag (event)=>void
                }
            }
        }
        */
        const ContextPadEntries = {
            'append.elementType' : {
                group,className,
                title,
                action:{
                    click: appendByClick(...prop[]),
                    dragstart: appendByDrag(...prop[])
                }
            }
        }
        return ContextPadEntries
    }
}
CustomContextPad.$inject = [
  'bpmnFactory',
  'config',
  'contextPad',
  'create',
  'elementFactory',
  'injector',
  'translate'
];
```





## customBpmnAdditionalModule

define

```js
import module from './module'
import module2 from './module2'
import module3 from './module3'
...;

export default {
    __init__ : [ 'moduleName' , 'moduleName2', 'moduleName3', ... ],
    moduleName:['type',module],
    moduleName2:[ 'type', module2 ],
    moduleName3:[ 'type', module3 ],
    ...
}
```

use

```js
import additionalModule from './...'
new BpmnModeler({
    ...,
    addtionalModules:[
    	additionalModule
    ]
})
```





## customModdleExtension 

define json 

```json
// eg
{
  "name": "QualityAssurance",
  "uri": "http://some-company/schema/bpmn/qa",
  "prefix": "qa",
  "xml": {
    "tagAlias": "lowerCase"
  },
  "types": [
    {
      "name": "AnalyzedNode",
      "extends": [
        "bpmn:FlowNode"
      ],
      "properties": [
        {
          "name": "suitable",
          "isAttr": true,
          "type": "Integer"
        }
      ]
    },
    {
      "name": "AnalysisDetails",
      "superClass": [ "Element" ],
      "properties": [
        {
          "name": "lastChecked",
          "isAttr": true,
          "type": "String"
        },
        {
          "name": "nextCheck",
          "isAttr": true,
          "type": "String"
        },
        {
          "name": "comments",
          "isMany": true,
          "type": "Comment"
        }
      ]
    },
    {
      "name": "Comment",
      "properties": [
        {
          "name": "author",
          "isAttr": true,
          "type": "String"
        },
        {
          "name": "text",
          "isBody": true,
          "type": "String"
        }
      ]
    }
  ],
  "emumerations": [],
  "associations": []
}

```



use

```js
import customModdleExtension  from './...'
new BpmnModeler({
    ...,
    addtionalModules:{
    	[ customModdleExPrefix: string ] : customModdleExtension
	}
})
```

