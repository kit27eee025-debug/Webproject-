C. Data Fusion Algorithm (RGB + Thermal)

# data_fusion.py
def fuse_detections(rgb_boxes, thermal_boxes, overlap_threshold=0.3):
    fused = []
    for rx, ry, rw, rh in rgb_boxes:
        for tx, ty, tw, th in thermal_boxes:
            # Compute intersection-over-union (IoU)
            xA, yA = max(rx, tx), max(ry, ty)
            xB, yB = min(rw, tw), min(rh, th)
            interArea = max(0, xB - xA) * max(0, yB - yA)
            boxAArea = (rw - rx) * (rh - ry)
            boxBArea = (tw - tx) * (th - ty)
            iou = interArea / float(boxAArea + boxBArea - interArea)
            if iou > overlap_threshold:
                fused.append((rx, ry, rw, rh))
    return fused
