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
