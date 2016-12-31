---
layout: post
title: My first Virtual Earth post
date: '2008-08-08 12:43:26'
---

So I've got a page with a virtual earth map into which I load a bunch of shapes. When the user mouses over a shape, I wanted the fill color to change and the infobox to popup. My initial plan was to use onmouseover and onmouseout but I found that when you mouse over the infobox, the onmouseout event fires. So instead of using the onmouseover and onmouseout events, I handled everything in onmousemove and keep track of my 'current' shape (_currentWatershed). I also check to see whether the shape the mouse is over is the same as the _currentWatershed and if so, I don't do anything.

<code><pre>
function mouseMoveHandler(e)
{
    if (e.elementID == null)
    {   //if we moused off all features 
        //and there was a _currentWatershed set
        if (_currentWatershed != null)
        {
            _currentWatershed.SetFillColor(_watershedsFillColor);
            _map.HideInfoBox(_currentWatershed);
            _currentWatershed = null;
        }
    }
    else
    {
        var shape = _map.GetShapeByID(e.elementID);
        if (shape != null && 
            shape.GetShapeLayer() == _watershedsLayer &&
                shape != _currentWatershed)
        {
            if (_currentWatershed != null) 
                _currentWatershed.SetFillColor(_watershedsFillColor);

            _currentWatershed = shape;
            _currentWatershed.SetFillColor(_watershedsHoverFillColor);
            _map.HideInfoBox();

            //shows the infobox at the mouse position when this event fires
            _map.ShowInfoBox(_currentWatershed, new VEPixel(e.mapX, e.mapY));
        }
    }
}
</pre></code>