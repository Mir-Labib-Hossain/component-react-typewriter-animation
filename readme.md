Use:
```jsx
const handleLoopEnds = () => {
  console.log("Hurray! Loop ends")
};

<Typewriter infinite={true} text="Freedom" interval={400} delay={0} onLoop={handleLoopEnds} />
```

Component:
```jsx
import { useEffect, useState } from "react";
import { TypeWriterAnimation } from "./styled";
type Props = {
  text: string;
  interval: number;
  infinite?: boolean;
  delay: number;
  onLoop?: () => void;
};

const Typewriter = ({ text, interval, infinite = false, delay, onLoop }: Props) => {
  const [currentText, setCurrentText] = useState("");
  const [currentIndex, setCurrentIndex] = useState(0);
  const [writingText, setWritingText] = useState(true);
  
  const [show, setShow] = useState(false);
  useEffect(() => {
    if (show) {
      let timeout: any;
      timeout = setTimeout(() => {
        if (writingText) {
          setCurrentText((prevText) => prevText + text[currentIndex]);
          setCurrentIndex((prevIndex) => prevIndex + 1);

          if (currentIndex == text.length - 1) {
            setWritingText(false);
          }
        } else if (infinite) {
          setCurrentText(currentText.substring(0, currentText.length - 1));
          setCurrentIndex((prevIndex) => prevIndex - 1);
          if (currentIndex == 1) {
            onLoop && onLoop();
            setWritingText(true);
          }
        }
      }, interval);
      return () => clearTimeout(timeout);
    }
  }, [currentIndex, interval, infinite, text, show]);

  useEffect(() => {
    if (show === false) {
      let timer2 = setTimeout(() => setShow(true), delay);

      return () => {
        clearTimeout(timer2);
      };
    }
  }, []);
  return <TypeWriterAnimation>{currentText}</TypeWriterAnimation>;
};

export default Typewriter;
```

Css:
```css
import { styled } from "styled-components";

export const TypeWriterAnimation = styled.span`
  border-right: 2px solid green;
  animation: blinkTextCursor 700ms steps(44) infinite normal;
  @keyframes blinkTextCursor {
    from {
      border-right-color: green;
    }
    to {
      border-right-color: transparent;
    }
  }
`;
```
